---
tags:
  - 计算机
  - JAVA
  - 大数据
  - YARN
---
代替JobTracker实现资源分配
Master端：
- ResourceManager 资源管理
- ApplicationMaster 任务调度+监控
Slave端：
- NodeManger代替TaskTracker实现任务处理