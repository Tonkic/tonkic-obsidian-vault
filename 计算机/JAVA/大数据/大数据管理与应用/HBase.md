HBase目标是处理非常庞大的表，需要在hadoop的基础上运行
### 存储模式
关系数据库是基于行模式存储的。HBase是基于列存储的，不同列族的文件是分离的
只有一个数据索引：行键
HBase中需要根据行键、列族、列限定符和时间戳来确定一个单元格，可以视为一个“四维坐标”，即行键，列族，列限定符（列族中具体的一列），时间戳
### 主服务器Master
主服务器Master负责管理和维护HBase表的分区信息，维护Region服务器列表，分配Region，负载均衡
### Region
Region和HDFS中的块差不多意思，默认大概128M，现代一般1G-2G，每个Region服务器存储10-1000个Region
### Region服务器
Region服务器负责存储和维护分配给自己的Region，处理来自客户端的读写请求
#### 读取过程
客户端并不依赖Master，而是通过Zookeeper获得Region的存储位置信息后，直接从Region服务器上读取数据，大多数客户端甚至从来不和Master通信，这种设计方式使得Master负载很小
#### 通过Zookeeper定位Region
##### .META.表
存储了Region和Region服务器的映射关系
当HBase表很大时，.META.表也会被分裂成多个Region
##### -ROOT-表
根数据表，又名-ROOT-表，记录所有元数据的具体位置
-ROOT-表只有唯一一个Region，名字是在程序中被写死的
Zookeeper文件记录了-ROOT-表的位置
#### Region服务器工作原理
系统会周期性地把MemStore缓存里的内容刷写到磁盘的StoreFile文件中，清空缓存，并在Hlog里面写入一个标记。
Hlog类似EditLog，每次启动检查标记，如果有更新先写入缓存MemStore，再写到StoreFile
每次刷写都生成一个新的StoreFile文件，因此，每个Store包含多个StoreFile文件，当StoreFile数量超过阈值时，将多个合并为一个，单个StoreFile过大时，又触发分裂操作，1个父Region被分裂成两个子Region