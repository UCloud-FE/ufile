# Quick Start
- [Video Tutorial](#Video Tutorial)
- [Instructions for use](# Instructions for use)
- [Parameter description](# Parameter description)
- [Hadoop Configuration](# Hadoop Configuration)
- [Usage](# Usage)
- [Scenario example](#Scenario example)
## Video Tutorial
Watch the following video to quickly get started with US3Hadoop
<video id="video" length=1000 width=800 controls="" preload="none" poster=" https://static.ucloud.cn/1a66ee246c0ff52203d82c9f001b6b45.png  ">
<source id="mp4" src=" http://caozuozhinan.cn-bj.ufileos.com/ Screen recording 2 US3 hadoop.mp4 ">
</video>
## Usage Notes
### Parameter Description
| Attribute Key | Attribute Description | Default |
| ------------------------------- | ------------------------------------------------------------ | ----------- |
| fs.us3.impl | US3's implementation class name for FileSystem, please make sure it is :cn.ucloud.us3.fs. US3FileSystem | None |
| fs.AbstractFileSystem.us3.impl | The class name of the implementation of AbstractFileSystem, fixed to cn.ucloud.us3.fs.US3Fs. |  None |
| fs.us3.access. key | The API public key or Token public key to access US3 | None |
| fs.us3.secret. key | Access to US3's API private key or Token private key | None | fs.us3.access.key
| fs.us3.socket.recv. buffer | US3 read stream buffer, in HiveSQL, Spark-SQL scenarios, if the data format is ORC, Parquet, etc., it is recommended to turn down this parameter, e.g. 16348(16KB), if the data format is normal text format, it is recommended to turn up this parameter, e.g. 1048576(16KB).  parameter, such as 1048576(1MB) | 65536(64KB) |
| fs.us3.log. level | US3 big data adapter log level, support info, error, debug, trace level.  Currently the logs are output to standard output | info | fs.us3.log.level
| fs.us3.endpoint | US3 intranet domain name suffix, such as: ufile.cn-north-02.ucloud.cn.  Refer to [locale and domain name]( https://docs.ucloud.cn/ufile/introduction/region ), the domain name should remove the `www.`  prefix. . |  none |
| fs.us3.async.wio. use | Whether to use asynchronous IO for single stream writes, which can improve write speed, especially effective for single large files, but will consume some CPU resources and may reduce task parallelism | false |
| fs.us3.async.wio. parallel | Effective when fs.us3.async.wio. use is true, indicating maximum parallelism for single file writes to a 4MB slice | 2 |
| fs.us3.metadata. Use | whether to enable the index cache server to accelerate the US3 index performance. This service needs user management and is currently in public beta | false|
| fs.us3.metadata. Host | in fs.us3. host, the user can also specify a custom domain name. You need to configure the resolution address in /etc/hosts or configure DNS resolution. This parameter is under test| None|
| fs.us3.metadata.zookeeper. Addrs | the zoomeeper node information used in the highly available VMDS cluster scheme. The parameter format is the zoomeeper server address and port number divided by "," for example: uhadoop-q4mbrxk2-master1:2181, uhadoop-q4mbrxk2-master2:2181, uhadoop-q4mbrxk2-core1:2181. If you configure the fs.us3.metadata.host parameter, this parameter is invalid** This function requires US3 adapter version > =1.3.0. * *| None|
| fs.us3.generate. MD5 | the default is' false '. If it is' true', when writing US3, the client calculates an MD5, and finally writes it to the metadata of the file with 'MD5 hash' as the key and Base64 encoded MD5 value as value. Enabling will increase the write delay by > =30%< br> **This feature requires US3 adapter version >= 1.0.2** | false |
### Hadoop configuration
one
You need to configure the above relevant configuration items in core-site. xml, e.g.
```xml
<? xml version="1.0" encoding="UTF-8"?>
<? xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.  See accompanying LICENSE file.
-->
<configuration>
<property>
<name>fs.defaultFS</name>
<value> hdfs://192.168.0.221:9000 </value>
</property>
......
<property>
<name>dfs.datanode.data.dir</name>
<value>/data/hd3/datanode</value>
</property>
<property>
<name>fs.us3.impl</name>
<value>cn.ucloud.us3.fs.US3FileSystem</value>
</property>
<property>
<name>fs.AbstractFileSystem.us3.impl</name>
<value>cn.ucloud.us3.fs.US3Fs</value>
</property>
<property>
<name>fs.us3.access.key</name>
<value>TOKEN_ xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</value>
</property>
<property>
<name>fs.us3.secret.key</name>
<value>xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</value>
</property>
<property>
<name>fs.us3.socket.recv.buffer</name>
<value>16348</value>
</property>
<property>
<name>fs.us3.thread.pool.size</name>
<value>8</value>
</property>
<property>
<name>fs.us3.log.level</name>
<value>info</value>
</property>
<property>
<name>fs.us3.endpoint</name>
<value>ufile.cn-north-02.ucloud.cn</value>
</property>
<property>
<name>fs.us3.async.wio.use</name>
<value>false</value>
</property>
<property>
<name>fs.us3.async.wio.parallel</name>
<value>2</value>
</property>
<property>
<name>fs.us3.metadata.use</name>
<value>false</value>
</property>
<property>
<name>fs.us3.metadata.host</name>
<value></value>
</property>
<property>
<name>fs.us3.metadata.zookeeper.addrs</name>
<value>uhadoop-q4mbrxk2-master1:2181,uhadoop-q4mbrxk2-master2:2181,uhadoop-q4mbrxk2-core1:2181</value>
</property>
......
</configuration>
```
### Usage
#### Hadoop file system access
Use the format `hadoop fs -ls us3://<bucket name>/<path>`.  Use the following pattern:
```
[ root@task10  ~]# hadoop fs -ls us3://bigdata-us3/HiBench/Aggregation/Input/rankings | more
Found 10241 items
-rw-r--r--   1 root supergroup          0 2021-01-26 13:46 us3://bigdata-us3/HiBench/Aggregation/Input/rankings/_ SUCCESS
-rw-r--r--   1 root supergroup    6375702 2021-01-26 13:30 us3://bigdata-us3/HiBench/Aggregation/Input/rankings/part-00000
-rw-r--r--   1 root supergroup    6379753 2021-01-26 13:30 us3://bigdata-us3/HiBench/Aggregation/Input/rankings/part-00001
-rw-r--r--   1 root supergroup    6372962 2021-01-26 13:30 us3://bigdata-us3/HiBench/Aggregation/Input/rankings/part-00002
......
```
**other use the same way as hdfs operation**
#### Hive using US3
When creating an external table, specify the location as the corresponding US3 path, such as `... location 'us3://<bucket name>/hive/tpc-ds/1002-orc/customer'`
#### Spark using US3
When reading or writing US3, whether via spark-submit, pyspark, spark-shell, spark-sql, etc., you need to specify the file path in the format: `us3://<bucket name>/path`
#### Flink using US3
simply specifying the path in US3 format.
**Note**
- file objects that already have storage will return default permissions.
- File names containing special characters, such as `:`, are not supported.
## Example scenario
Suppose you need to backup the /var directory data of the HDFS cluster to the /distcp/ prefix of the bigdata-us3 storage of US3, you can perform a copy via hadoop's own distcp after installing the previous adapter, such as
```
[ root@master1  tools]# hadoop distcp -bandwidth 15 -m 20 -skipcrccheck -update -pugp  /var us3://bigdata-us3/distcp/var
21/01/26 16:51:50 INFO tools. DistCp: Input Options: DistCpOptions{atomicCommit=false, syncFolder=true, deleteMissing=false, ignoreFailures=false, overwrite=false, skipCRC=true, blocking=true, numListstatusThreads=0, maxMaps=20, mapBandwidth=15, sslConfigurationFile='null', copyStrategy='uniformsize', preserveStatus=[USER, GROUP, PERMISSION] , preserveRawXattrs=false, atomicWorkPath=null, logPath=null, sourceFileListing=null, sourcePaths=[/var], targetPath=us3://bigdata-us3/distcp/var, targetPathExists=false, filtersFile='null'}
21/01/26 16:51:51 INFO tools. SimpleCopyListing: Paths (files+dirs) cnt = 9226;  dirCnt = 851
21/01/26 16:51:51 INFO tools. SimpleCopyListing: Build file listing completed.
21/01/26 16:51:51 INFO Configuration. deprecation: io.sort.mb is deprecated.  Instead, use mapreduce.task.io.sort.mb
21/01/26 16:51:51 INFO Configuration. deprecation: io.sort.factor is deprecated.  Instead, use mapreduce.task.io.sort.factor
21/01/26 16:51:51 INFO tools. DistCp: Number of paths in the copy list: 9226
21/01/26 16:51:52 INFO tools. DistCp: Number of paths in the copy list: 9226
21/01/26 16:51:53 INFO mapreduce. JobSubmitter: number of splits:25
21/01/26 16:51:53 INFO mapreduce. JobSubmitter: Submitting tokens for job: job_ 1611108331895_ 0847
21/01/26 16:51:54 INFO impl. YarnClientImpl: Submitted application application_ 1611108331895_ 0847
21/01/26 16:51:54 INFO mapreduce. Job: The url to track the job:  http://master2:23188/proxy/application_1611108331895_0847/
21/01/26 16:51:54 INFO tools. DistCp: DistCp job-id: job_ 1611108331895_ 0847
21/01/26 16:51:54 INFO mapreduce. Job: Running job: job_ 1611108331895_ 0847
21/01/26 16:52:01 INFO mapreduce. Job: Job job_ 1611108331895_ 0847 running in uber mode : false
21/01/26 16:52:01 INFO mapreduce. Job:  map 0% reduce 0%
21/01/26 16:52:18 INFO mapreduce. Job:  map 1% reduce 0%
21/01/26 16:52:19 INFO mapreduce. Job:  map 3% reduce 0%
21/01/26 16:52:20 INFO mapreduce. Job:  map 6% reduce 0%
21/01/26 16:53:07 INFO mapreduce. Job:  map 7% reduce 0%
21/01/26 16:53:31 INFO mapreduce. Job:  map 8% reduce 0%
...
```
**Note**
- `-p` parameter without `b`, because the actual block size of US3 is not consistent with HDFS, and re-executing the command will cause existing files to be overwritten and uploaded again.
- `-skipcrccheck -update` should be turned on, because US3 integrity checks are done through a different mechanism.
If you do not want to install the adapter to perform backup or file system operations (to avoid the risk of intruding into existing services), you can perform the operation in the following format:
```
hadoop fs|distcp|... -Dfs.us3.impl=cn.uclo