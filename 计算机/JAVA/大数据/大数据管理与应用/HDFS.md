## NameNode
主节点 = Master Node = NameNode
### NameNode用于管理NameSpace
NameSpace = FsImage + EditLog
#### FsImage
FsImage维护文件目录树，从根目录找到叶子，叶子结点是一个inode，记录了文件有多少块，文件权限，修改时间等信息；FsImage是在内存中的NameNode某一个时期的快照，储存在磁盘
#### EditLog
EditLog记录所有文件的创建、删除、重命名等操作的日志
##### 解决运行期间EditLog不断变大的问题
使用SecondaryNameNode，SecondaryNameNode定期向NameNode发出中断，使其别用EditLog，用着edit.new先
SecondaryNameNode通过HTTP GET得到FsImage 和 EditLog，帮NameNode合并之后Post给它，此时NameNode不中断服务
## DataNode
从节点 = Slave Node = DataNode

默认一个块64M，文件分块处理

### 冗余副本存取策略
第一个副本：放置在上传文件的数据节点；如果是集群外提交，则随机挑选一台磁盘不太满、CPU不太忙的节点
第二个副本：放置在与第一个副本不同的机架的节点上
第三个副本：与第一个副本相同机架的其他节点上
## HDFS HA
目的是解决单点故障，设置两个NameNode，一个Active一个Standby（待命）
## HDFS Federation
设置多个Namenode，互相隔离，不需要协调通信，所有Namenode共享数据节点
