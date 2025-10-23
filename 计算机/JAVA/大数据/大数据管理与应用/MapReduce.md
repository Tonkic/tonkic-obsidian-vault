MapReduce框架采用了Master/Slave架构，包括一个Master和若干个Slave。
Master上运行JobTracker，Slave上运行TaskTracker
#### 组成
- Client
- JobTracker（分配任务，通过TaskScheduler获得TaskTracker的信息）
- TaskTracker（执行任务的节点，通过HeartBeat报告状态，资源（CPU、内存等）会分为一个一个slot，task获取slot再执行）
- Task（具体任务，map和reduce，不同的Task之间没有通信）
#### 执行步骤
- split：将大数据划分为小数据
- map：一个split一个map任务，理想的分片大小是一个HDFS块
- shuffle：
	- map端：如果溢写数量大于设定值则 分区 排序 合并后Merge，否则直接Merge（在Map任务之后）
		- “溢写”是指当内存缓冲区的数据量超过阈值时，将数据从内存写入磁盘的过程
	- reduce端：将来自不同map机器的先Merge后Combine（在reudce任务之前）
- reduce：数量小于等于slot数目，一般预留一些空slot

###### 合并（Combine）和归并（Merge）的区别:
两个键值对<“a”，1>和<“a”，1>，如果合并，会得到<“a”，2>，如果归并，会得到<“a”，<1,1>>

