这类 palace structure 的实际落地，本质上就是 metadata filtering，使用四层结构(wings, rooms, closets, drawers)

| Layer  层 | What  什么                                                               | Size  大小          | When  何时                        |
| -------- | ---------------------------------------------------------------------- | ----------------- | ------------------------------- |
| **L0**   | Identity — who is this AI?  <br>身份——这是哪个 AI？                           | ~50 tokens        | Always loaded  始终加载             |
| **L1**   | Critical facts — team, projects, preferences  <br>关键事实——团队、项目、偏好       | ~120 tokens(AAAK) | Always loaded  始终加载中            |
| **L2**   | Room recall — recent sessions, current project  <br>房间回忆 — 最近的会话，当前项目  | On demand  按需     | When topic comes up  <br>当话题出现时 |
| **L3**   | Deep search — semantic query across all closets  <br>深度搜索——跨所有储物间的语义查询 | On demand  按需     | When explicitly asked  在明确要求时   |