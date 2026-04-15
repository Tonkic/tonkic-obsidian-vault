### windows
打开`C:/Users/你的用户名/.claude/settings.json`
```bash
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "sk开头的 api key",
    "ANTHROPIC_BASE_URL": "https://api.com",
    "ANTHROPIC_MODEL": "claude-sonnet-4-6"
  },
  "includeCoAuthoredBy": false,
  "model": "Sonnet"
}
```

### Linux
编辑 ~/.bashrc或 ~/.zshrc,加入
```
export ANTHROPIC_AUTH_TOKEN="sk开头的 api key"
export ANTHROPIC_BASE_URL="https://api.com"
export ANTHROPIC_MODEL="claude-sonnet-4-6"
```