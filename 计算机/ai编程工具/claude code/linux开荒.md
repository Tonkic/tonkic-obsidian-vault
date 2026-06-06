```bash
#!/usr/bin/env bash
set -euo pipefail

CONFIG_JSON='{
  "env": {
    "ANTHROPIC_BASE_URL": "https://CPA或sub2api.com:12345",
    "ANTHROPIC_AUTH_TOKEN": "sk-Lxxxxxxx你的秘钥xxxxxxx",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1",
    "CLAUDE_CODE_ATTRIBUTION_HEADER": "0"
  },
  "permissions": {
    "allow": [
      "*"
    ],
    "defaultMode": "bypassPermissions"
  },
  "skipDangerousModePermissionPrompt": true
}'
CLAUDE_NPM_PACKAGE="@anthropic-ai/claude-code"
BASE_APT_PACKAGES=(tmux curl ca-certificates gnupg)
MIN_NODE_MAJOR=18
NODESOURCE_SETUP_URL="https://deb.nodesource.com/setup_current.x"
GLOBAL_CONFIG_DIR="/claude"
GLOBAL_CONFIG_FILE="/claude/setting.json"
USER_CONFIG_DIR="$HOME/.claude"
USER_CONFIG_FILE="$HOME/.claude/settings.json"
BASHRC_FILE="$HOME/.bashrc"
BASHRC_MARKER_START="# >>> cc-ssh-tmux >>>"
BASHRC_MARKER_END="# <<< cc-ssh-tmux <<<"
TMUX_SESSION_NAME="cc"

if [[ "${EUID}" -eq 0 ]]; then
  SUDO=""
else
  if command -v sudo >/dev/null 2>&1; then
    SUDO="sudo"
  else
    echo "[ERROR] 请用 root 运行，或先安装 sudo。"
    exit 1
  fi
fi

echo "[1/6] 安装基础依赖（tmux / curl / ca-certificates / gnupg）..."
$SUDO apt-get update -y
$SUDO apt-get install -y --no-install-recommends "${BASE_APT_PACKAGES[@]}"

CURRENT_NODE_MAJOR=0
if command -v node >/dev/null 2>&1; then
  CURRENT_NODE_MAJOR="$(node -v | sed -E 's/^v([0-9]+).*/\1/')"
fi

if [[ "$CURRENT_NODE_MAJOR" -lt "$MIN_NODE_MAJOR" ]]; then
  echo "[2/6] Node.js 版本 < ${MIN_NODE_MAJOR}，升级到最新版本..."
  if [[ -n "$SUDO" ]]; then
    curl -fsSL "$NODESOURCE_SETUP_URL" | $SUDO -E bash -
  else
    curl -fsSL "$NODESOURCE_SETUP_URL" | bash -
  fi
  $SUDO apt-get install -y --no-install-recommends nodejs
else
  echo "[2/6] Node.js 版本满足要求（v${CURRENT_NODE_MAJOR}），跳过升级。"
fi

echo "[3/6] npm 全局安装 Claude Code..."
if ! command -v claude >/dev/null 2>&1; then
  $SUDO npm install -g "$CLAUDE_NPM_PACKAGE"
else
  echo "[INFO] 已检测到 claude，跳过安装。"
fi

echo "[4/6] 写入 Claude 配置..."
$SUDO mkdir -p "$GLOBAL_CONFIG_DIR"
printf '%s\n' "$CONFIG_JSON" | $SUDO tee "$GLOBAL_CONFIG_FILE" >/dev/null

mkdir -p "$USER_CONFIG_DIR"
printf '%s\n' "$CONFIG_JSON" > "$USER_CONFIG_FILE"

echo "[5/6] 配置 ~/.bashrc（SSH 下 tmux + -c + 最高权限）..."
touch "$BASHRC_FILE"

if grep -qF "$BASHRC_MARKER_START" "$BASHRC_FILE"; then
  awk -v start="$BASHRC_MARKER_START" -v end="$BASHRC_MARKER_END" '
    BEGIN { skip=0 }
    $0==start { skip=1; next }
    $0==end { skip=0; next }
    skip==0 { print }
  ' "$BASHRC_FILE" > "${BASHRC_FILE}.tmp"
  mv "${BASHRC_FILE}.tmp" "$BASHRC_FILE"
fi

cat >> "$BASHRC_FILE" <<EOF
$BASHRC_MARKER_START
unalias cc 2>/dev/null
cc() {
    local _base="IS_SANDBOX=1 command claude --dangerously-skip-permissions"

    if [[ -n "\$SSH_CONNECTION" && -z "\$TMUX" ]] && command -v tmux >/dev/null 2>&1; then
        if [ "\$#" -gt 0 ]; then
            local _args
            printf -v _args '%q ' "\$@"
            tmux new-session -A -s "$TMUX_SESSION_NAME" "\${_base} \${_args}"
        else
            tmux new-session -A -s "$TMUX_SESSION_NAME" "bash -lc '\${_base} -c || \${_base}'"
        fi
        return
    fi

    if [ "\$#" -gt 0 ]; then
        IS_SANDBOX=1 command claude --dangerously-skip-permissions "\$@"
    else
        IS_SANDBOX=1 command claude --dangerously-skip-permissions -c || IS_SANDBOX=1 command claude --dangerously-skip-permissions
    fi
}
$BASHRC_MARKER_END
EOF

echo "[6/6] 完成。"
echo ""
echo "请执行：source ~/.bashrc"
echo "之后在 SSH 里直接输入 cc 即可：tmux + 自动续会话(-c) + 最高权限。"
```