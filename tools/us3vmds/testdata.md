#Performance data and analysis
Us3vmds is developed based on the go language and comes with the go profile function. You can start the go runtime performance analysis HTTP service at the specified address by using the following command:
```
➜  bin  ✗ us3vmds pprof --open 127.0.0.1 8081
➜  bin  ✗ netstat -tunpl | grep 8081
tcp        0      0 127.0.0.1:8081          0.0.0.0:*               LISTEN      3312/us3vmds
➜ bin ✗ us3vmds pprof --close / / close the runtime performance analysis HTTP service
➜  bin  ✗ netstat -tunpl | grep 8081
```
For the runtime analysis of go, please refer to the official document of go [package pprof]（ https://golang.org/pkg/net/http/pprof/ )
##Test environment
###Hardware environment
|Deployment service | quantity | operating system | CPU | memory | data disk (quantity / single disk capacity / specification)|
| ------------------------------------------------------------ | ---- | --------------- | ---- | ---- | ----------------------------- |
|Namenode<br>resourcemanager<>zookeeper<br>journalnode | 3 | CentOS 7.6 64 bit | 32c | 128G | 1/100g/rssd cloud disk|
|Datanode<br>nodemanager | 10 | CentOS 7.6 64 bit | 32c | 128G | 10/300g/rssd cloud disk|
###Software environment
|Software | version number | restrictions|
| --------- | ------ | ------------ |
|Hadoop | 2.8.5 | standard community version|
|Hive | 2.3.7 | standard community version|
|Spark | 2.4.3 | standard community version|
|Zookeeper | 3.5.6 | standard community version|
###Key parameters
#### Hadoop
|Configuration file | configuration item | configuration value|
| ------------- | --------------- | ------ |
| hdfs-site. xml | dfs. replication | 3      |
###Hibench test
Hibench is Intel's open source big data benchmarking tool, which can help evaluate different big data frameworks in terms of speed, throughput and system resource utilization. It includes a set of Hadoop, spark and stream workloads, including sort, wordcount, terasort, repartition, sleep, SQL, PageRank, nutch indexing, Bayes, kmeans, nweight and enhanced dfsio. It also contains some stream workloads for spark streams, Flink, storm, and gearpump. There are a total of 29 workloads in hibench. Workloads are divided into six categories: micro, ml (machine learning), SQL, graph, websearch, and stream.
The version of this hibench test is:
[v7.0.0]( https://github.com/Intel-bigdata/HiBench/tree/HiBench-7.0 )
The public configuration information is:
|Configuration file | configuration item | configuration value|
| ------------ | ----------------------------------- | ------ |
| hibench. conf | hibench.scale. profile               | mydata |
| hibench. conf | hibench.default.map. parallelism     | 40960  |
| hibench. conf | hibench.default.shuffle. parallelism | 10240  |
| spark. conf   | hibench.yarn.executor. num           | 50     |
| spark. conf   | hibench.yarn.executor. cores         | 4      |
| spark. conf   | spark.executor. memory               | 16g    |
| spark. conf   | spark.driver. memory                 | 8g     |
##Test data
Seven workloads were selected for the test, which were based on US3 (only adapter tools), us3vmds (adapter and us3vmds) and HDFS:
### Micro
**sort**
The test parameters are adjusted to:
- hibench.sort.mydata. datasize: 3000000000000
>This parameter represents the total data volume, about 3TB
Test data:
! [](/images/hibench-micro.sort.png)
**terasort**
The test parameters are adjusted to:
- hibench.sort.mydata. datasize: 30000000000
>This parameter indicates how many rows of data are generated. According to the source code, the size of each randomly generated row of data is 100 bytes, so in order to be consistent with the overall test data volume of micro.sort, it is set to 3000000000.
Test data:
! [](/images/hibench-micro.terasort.png)
**wordcoun**
The test parameters are adjusted to:
- hibench.dfsioe.mybigdata.read. number_ of_ files 44704
- hibench.dfsioe.mybigdata.read. file_ size 64 // 64MiB
- hibench.dfsioe.mybigdata.write. number_ of_ files 44704
- hibench.dfsioe.mybigdata.write. file_ size 64 // 64MiB
>The amount of read and write data tested is about 3TB
Test data:
! [](/images/hibench-micro.wordcount.png)
**dfsioe**
The test parameters are adjusted to:
- hibench.dfsioe.mybigdata.read. number_ of_ files 44704
- hibench.dfsioe.mybigdata.read. file_ size 64 // 64MiB
- hibench.dfsioe.mybigdata.write. number_ of_ files 44704
- hibench.dfsioe.mybigdata.write. file_ size 64 // 64MiB
>The amount of read and write data tested is about 3TB
Test data:
! [](/images/hibench-micro.dfsioe.png)
### SQL
**scan**
The test parameters are adjusted to:
- hibench.scan.mybigdata. uservisits 20000000000
- hibench.scan.mybigdata. pages 100000000
Test data:
! [](/images/hibench-sql.scan.png)
**join**
The test parameters are adjusted to:
- hibench.scan.mybigdata. uservisits 20000000000
- hibench.scan.mybigdata. pages 100000000
Test data:
! [](/images/hibench-sql.join.png)
>Note: the create ranks and create uservisits tasks are the same as sql.scan, so they are ignored
**aggregation**
The test parameters are adjusted to:
- hibench.scan.mybigdata. uservisits 20000000000
- hibench.scan.mybigdata. pages 100000000
Test data:
! [](/images/hibench-sql.aggregation.png)
>Note: the create ranks and create uservisits tasks are the same as sql.scan, so they are ignored
In general, the current scheme based on US3 has a certain gap compared with the native distributed storage HDFS, but the performance of the scheme based on us3vmds has been greatly improved in most scenarios, and will continue to improve in the future, launching a scheme comparable to or even surpassing HDFS.
#Defects
Although us3vmds can indeed bring further performance improvement to big data solutions based on US3 storage, there are still many defects in the current design of us3vmds, which are mainly reflected in several aspects:
1. Single point architecture reduces reliability. However, us3vmds is a stateless design. If us3vmds exits for some reasons, it will not cause the loss of the index. As long as it is restarted quickly, the materialized view of the directory tree can be rebuilt according to the index of US3. Generally, within 10s of us3vmds exiting during the service process, it will not affect the big data task, because the US3 big data adapter tool retries the error for a certain period of time. As long as the us3vmds is in the startup state in time, the reliability problems caused by the abnormal exit of us3vmds can be greatly reduced. Of course, because the node of us3vmds needs to be migrated due to failure, the service address of us3vmds needs to be updated, and the delay is relatively long. At this time, you can simply modify the configuration and temporarily change back to the scheme of using only the US3 big data adapter.
2. Some operations cannot guarantee atomicity. For example, moving directories and deleting directories in us3vmds will become related files under the directory prefix of batch operation, and any file in the middle may fail even with retry logic. At present, us3vmds can only solve such problems by trying again as much as possible.
3. When non big data clusters write data, the us3vmds index cannot guarantee strong consistency. Because the data written to US3 through non US3 big data adapters (such as SDK) cannot be notified to us3vmds in time, us3vmds can only rely on the big data cluster to actively trigger the synchronization operation to synchronize the latest index, which can only achieve weak consistency. Write the data of US3 through the US3 big data adapter in the us3vmds scheme. Since us3vmds will be actively notified after writing the US3 successfully, us3vmds can timely update the index information and achieve strong consistency.
Therefore, at present, us3vmds does not meet acid, but only base.