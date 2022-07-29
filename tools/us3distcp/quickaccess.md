
# Quick start

- [Parameter description](# Parameter description)
- [Use Sync](# Use Sync)
- [Use Checksum](# Use Checksum)
- [Incremental migration](# Incremental migration)
- [Defects](#defects)

## Parameter description

```shell
hadoop jar us3-bigdata-adaptor-2.8.5-1.1.0.jar cn.ucloud.us3.fs.distcp.
usage: distcp OPTIONS [source_path...] <target_path>
              OPTIONS
 -workspace <arg> the workspace keeps a directory of state data for the
                    entire verification synchronization phase
 -modtime check the modification time.when the modification time of the source end is later than the
                    when the modification time of the source end is later than the destination end,
                    when the modification time of the source end is later than the destination end, it is considered that it needs to be copied
 -checksum <arg> to calculate the checksum, 'randome' only compares the
                    head and tail 4MB checksums, and randomly takes 4 1MB
                    fragments in the middle of the file for verification.
                    'all' will calculate the checksum of the entire file
                    and compare it. not on by default
 -algorithm <arg> checksum algorithm, the default is crc32, you can also
                    choose 'md5'
 -dump <arg> Display the corresponding input information, input,
                    checkout, cpout
 -enforce If the status result of the check or copy exists,
                    If the status result of the check or copy exists, whether to enforce the check or copy
 -onlycheck Only get the list of files that fail the integrity
                    check, no need to copy
 -skipcheck skip all checks. including file size, modification
                    time, checksum

```

-workspace: This parameter is required, it will create `input`, `checkout` and `cpout` directories in the directory specified by this parameter, the three directories keep some status information of each phase, `input` is responsible for keeping the input information of the checkout phase, which is converted from the input parameters of the execution program, ` checkout` is responsible for keeping the result of the file to be synchronized output in the checkout phase and used as input in the synchronization phase, `cpout` keeps the file that failed in the synchronization phase, and if the record in the file is empty, the whole synchronization is proved to be successful.
-modtime: optional, if enabled it will enable the `destination modification time is up to date` checksum policy.
-checksum: optional, if enabled, enables the checksum policy of `checksum checksum`, whose value is `all` for a full checksum of the file contents and `randome` for a sample checksum of the file contents.
-algorithm: optional, this parameter is used to select the checksum algorithm, currently only `md5`, `crc32c` are provided, if you consider the checksum time delay, you can use `crc32c`, the default is `crc32c`
-dump: list the information in `input`, `checkout`, `cpout` in workspace.
-enforce: generally cannot be executed again after a checkout or synchronization phase has been performed, but this parameter can force checkout and synchronization to be performed again.
-onlycheck: this parameter will have only a check phase, not a synchronization phase.
-skipcehck: this parameter will skip the check phase and perform only the synchronization phase.

In addition the following key parameters of the adapter need to be passed through the `-D` option of GenericOptionsParser:

- fs.us3.imp: always `cn.ucloud.us3.fs.US3FileSystem`.
- fs.us3.access.key: API or Token public key.
- fs.us3.secret.key: API or Token private key.
- fs.us3.endpoint: US3 intranet domain suffix, e.g.:ufile.cn-north-04.ucloud.cn. refer to [locale and domain](https://docs.ucloud.cn/ufile/introduction/region), domain name needs to remove the `www.` prefix .
- fs.us3.async.wio.use: Whether to use asynchronous IO for single stream writes, can improve write speed, especially effective for single large files, but will consume some CPU resources, may reduce task parallelism, default is `false`.
- fs.us3.async.wio.parallel: Effective if fs.us3.async.wio.use is true, indicates the maximum parallelism for a single file write of 4MB slice, default is 2.
- fs.us3.metadata.use: Whether to enable the index cache server to speed up US3 index performance, this service requires user management and is currently in public beta.
- fs.us3.metadata.host: Effective in the case of fs.us3.metadata.usetrue, the IP:Port address form can be specified directly, this approach does not require configuration of the /etc/hosts file. Users can also specify a custom domain name, which requires configuring the resolution address in /etc/hosts or configuring DNS resolution. This parameter is under test.
- fs.us3.generate.md5: default is `false`, if it is `true` then when writing to US3, the client will calculate an MD5 and write it to the metadata of the file with `md5-hash` as the key and md5 value as the value at the end. Enabling it increases the write delay. This feature requires US3 adapter version >= 1.0.2
- mapreduce.job.user.classpath.first: always `true`.
- mapreduce.task.classpath.user.precedence: always `true`.

Finally, pass in the adapter jar package path via the `-libjars` option.

## Use synchronization

For example, synchronize the files in the `/var/log/hadoop-yarn/apps/root/logs/application_1613870792268_0067 directory` to the `hdfs_backup` prefix of the us3 storage `bigdata-us3`.

```shell
➜ bin ✗ hadoop jar . /us3-bigdata-adaptor-2.8.5-1.1.0.jar \
cn.ucloud.us3.fs.distcp.
-Dfs.us3.impl=cn.ucloud.us3.fs.US3FileSystem \
-Dfs.us3.access.key=TOKEN_7468973e-d192-4378-8253-xxxxxx \
-Dfs.us3.secret.key=9a632eb8-3938-43a0-a7e1-xxxxxx \
-Dfs.us3.endpoint=internal-cn-bj.ufileos.com \
-Dfs.us3.async.wio.use=false \
-Dfs.us3.async.wio.parallel=2 \
-Dfs.us3.metadata.use=true \
-Dmapreduce.job.user.classpath.first=true \
-Dmapreduce.task.classpath.user.precedence=true \
-libjars . /us3-bigdata-adaptor-2.8.5-1.1.0.jar \
-workspace /workspace/us3distcptest /var/log/hadoop-yarn/apps/root/logs/application_1613870792268_0067 us3://bigdata-us3/hdfs_backup
21/03/03 16:28:54 INFO distcp.DistCp: Prepare to enter data, on workspace:/workspace/us3distcptest
21/03/03 16:28:54 INFO distcp.DistCp: Prepare to enter data by sources:
21/03/03 16:28:54 INFO distcp.DistCp: - /var/log/hadoop-yarn/apps/root/logs/application_1613870792268_0067
21/03/03 16:28:54 INFO distcp.DistCp: Prepare to enter data by target: us3://bigdata-us3/hdfs_backup
21/03/03 16:28:54 INFO distcp.CopyListing: check input file:/workspace/us3distcptest/input/input
21/03/03 16:28:55 INFO distcp.DistCp: us3 distcp need check size/count is 10
21/03/03 16:28:55 INFO distcp.DistCp: do check job, num of maps:1
21/03/03 16:28:55 INFO distcp.DistCpInputFormat: the size of files count by DistCp, size is 10
21/03/03 16:28:55 INFO distcp.DistCpInputFormat: Average size of map: 10, Number of maps:1, total size:10
21/03/03 16:28:56 INFO mapreduce.JobSubmitter: number of splits:1
21/03/03 16:28:56 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1614496134003_0032
21/03/03 16:28:56 INFO impl.YarnClientImpl: Submitted application application_1614496134003_0032
21/03/03 16:28:56 INFO mapreduce.Job: The url to track the job: http://master2:23188/proxy/application_1614496134003_0032/
21/03/03 16:28:56 INFO mapreduce.Job: Running job: job_1614496134003_0032
21/03/03 16:29:02 INFO mapreduce.Job: Job job job_1614496134003_0032 running in uber mode : false
21/03/03 16:29:02 INFO mapreduce.Job: map 0% reduce 0%
21/03/03 16:29:07 INFO mapreduce.Job: map 100% reduce 0%
21/03/03 16:29:14 INFO mapreduce.Job: map 100% reduce 100%
21/03/03 16:29:14 INFO mapreduce.Job: Job job_1614496134003_0032 completed successfully
21/03/03 16:29:14 INFO mapreduce.Job: Counters: 56
File System Counters
FILE: Number of bytes read=0
FILE: Number of bytes written=350391
FILE: Number of read operations=0
FILE: Number of large read operations=0
FILE: Number of write operations=0
HDFS: Number of bytes read=1853
HDFS: Number of bytes written=1867
HDFS: Number of read operations=17
HDFS: Number of large read operations=0
HDFS: Number of write operations=2
US3: Number of bytes read=0
US3: Number of bytes written=0
US3: Number of read operations=0
US3: Number of large read operations=0
US3: Number of write operations=0
Job Counters
Launched map tasks=1
Launched reduce tasks=1
Other local map tasks=1
Total time spent by all maps in occupied slots (ms)=193914
Total time spent by all reduces in occupied slots (ms)=329522
Total time spent by all map tasks (ms)=3078
Total time spent by all reduce tasks (ms)=2701
Total vcore-milliseconds taken by all map tasks=3078
Total vcore-milliseconds taken by all reduce tasks=2701
Total megabyte-milliseconds taken by all map tasks=6192936
Total megabyte-milliseconds taken by all reduce tasks=10544704
Map-Reduce Framework
Map input records=10
Map output records=10
Map output bytes=1502
Map output materialized bytes=1528
Input split bytes=116
Combine input records=0
Combine output records=0
Reduce input groups=10
Reduce shuffle bytes=1528
Reduce input records=10
Reduce output records=10
Spilled Records=10
Shuffled Maps =1
Failed Shuffles=0
Merged Map outputs=1
GC time elapsed (ms)=190
CPU time spent (ms)=1700
Physical memory (bytes) snapshot=1480015872
Virtual memory (bytes) snapshot=8427098112
Total committed heap usage (bytes)=2974810112
Shuffle Errors
BAD_ID=0
CONNECTION=0
IO_ERROR=0
WRONG_LENGTH=0
WRONG_MAP=0
WRONG_REDUCE=0
cn.ucloud.us3.fs.distcp.DistCpMapper$US3_DISTCP_CHECK
NUMBER_OF_DST_NOT_FOUND=10
TOTAL_NUMBER_OF_INPUT_FILES=10
File Input Format Counters
Bytes Read=1737
File Output Format Counters
Bytes Written=1867
21/03/03 16:29:14 INFO distcp.DistCp: us3 distcp check taken: 0 Days 0 Hours 0 Minutes 11 Seconds 893 Milliseconds
21/03/03 16:29:14 WARN distcp.CopyListing: hdfs://Ucluster/workspace/us3distcptest/checkout/_SUCCESS read err:hdfs://Ucluster/workspace/ us3distcptest/checkout/_SUCCESS not a SequenceFile
21/03/03 16:29:14 ERROR distcp.DistCp: us3 distcp, some files failed the integrity check, need to copy ~ o(╥﹏╥)o
21/03/03 16:29:14 WARN distcp.CopyListing: hdfs://Ucluster/workspace/us3distcptest/checkout/_SUCCESS read err:hdfs://Ucluster/workspace/ us3distcptest/checkout/_SUCCESS not a SequenceFile
21/03/03 16:29:14 INFO distcp.DistCp: us3 distcp need copy size is 5063546
21/03/03 16:29:14 INFO distcp.DistCp: calculate map num, total size:5063546 ,size_per_task:4294967296
21/03/03 16:29:14 INFO distcp.DistCp: calculate map num:1
21/03/03 16:29:14 INFO distcp.DistCp: do copy job, num of maps:1
21/03/03 16:29:15 INFO distcp.DistCpInputFormat: the size of files count by DistCp, size is 5063546
21/03/03 16:29:15 INFO distcp.DistCpInputFormat: Average size of map: 5063546, Number of maps:1, total size:5063546
21/03/03 16:29:15 WARN distcp.DistCpInputFormat: hdfs://Ucluster/workspace/us3distcptest/checkout/_SUCCESS read err:hdfs://Ucluster/ workspace/us3distcptest/checkout/_SUCCESS not a SequenceFile
21/03/03 16:29:15 INFO mapreduce.JobSubmitter: number of splits:1
21/03/03 16:29:15 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1614496134003_0033
21/03/03 16:29:15 INFO impl.YarnClientImpl: Submitted application application_1614496134003_0033
21/03/03 16:29:15 INFO mapreduce.Job: The url to track the job: http://master2:23188/proxy/application_1614496134003_0033/
21/03/03 16:29:15 INFO mapreduce.Job: Running job: job_1614496134003_0033
21/03/03 16:29:23 INFO mapreduce.Job: Job job_1614496134003_0033 running in uber mode : false
21/03/03 16:29:23 INFO mapreduce.Job: map 0% reduce 0%
21/03/03 16:29:30 INFO mapreduce.Job: map 100% reduce 0%
21/03/03 16:29:34 INFO mapreduce.Job: map 100% reduce 100%
21/03/03 16:29:35 INFO mapreduce.Job: Job job_1614496134003_0033 completed successfully
21/03/03 16:29:35 INFO mapreduce.Job: Counters: 57
File System Counters
FILE: Number of bytes read=0
FILE: Number of bytes written=348845
FILE: Number of read operations=0
FILE: Number of large read operations=0
FILE: Number of write operations=0
HDFS: Number of bytes read=5065539
HDFS: Number of bytes written=85
HDFS: Number of read operations=27
HDFS: Number of large read operations=0
HDFS: Number of write operations=2
US3: Number of bytes read=0
US3: Number of bytes written=5063546
US3: Number of read operations=0
US3: Number of large read operations=0
US3: Number of write operations=0
Job Counters
Launched map tasks=1
Launched reduce tasks=1
Other local map tasks=1
Total time spent by all maps in occupied slots (ms)=298179
Total time spent by all reduces in occupied slots (ms)=321714
Total time spent by all map tasks (ms)=4733
Total time spent by all reduce tasks (ms)=2637
Total vcore-milliseconds taken by all map tasks=4733
Total vcore-milliseconds taken by all reduce tasks=2637
Total megabyte-milliseconds taken by all map tasks=9522796
Total megabyte-milliseconds taken by all reduce tasks=10294848
Map-Reduce Framework
Map input records=10
Map output records=0
Map output bytes=0
Map output materialized bytes=6
Input split bytes=126
Combine input records=0
Combine output records=0
Reduce input groups=0
Reduce shuffle bytes=6
Reduce input records=0
Reduce output records=0
Spilled Records=0
Shuffled Maps =1
Failed Shuffles=0
Merged Map outputs=1
GC time elapsed (ms)=148
CPU time spent (ms)=2660
Physical memory (bytes) snapshot=1519132672
Virtual memory (bytes) snapshot=8458067968
Total committed heap usage (bytes)=3032481792
Shuffle Errors
BAD_ID=0
CONNECTION=0
IO_ERROR=0
WRONG_LENGTH=0
WRONG_MAP=0
WRONG_REDUCE=0
cn.ucloud.us3.fs.distcp.DistCpMapper$US3_DISTCP_COPY
READ_SIZE_OF_SOURCE=5063546
TOTAL_NUMBER_OF_INPUT_FILES=10
WRITE_SIZE_OF_DISTINATION=5063546
File Input Format Counters
Bytes Read=1867
File Output Format Counters
Bytes Written=85
21/03/03 16:29:35 INFO distcp.DistCp: us3 distcp copy taken: 0 Days 0 Hours 0 Minutes 11 Seconds 872 Milliseconds
21/03/03 16:29:35 WARN distcp.CopyListing: hdfs://Ucluster/workspace/us3distcptest/cpout/_SUCCESS read err:hdfs://Ucluster/workspace/ us3distcptest/cpout/_SUCCESS not a SequenceFile
21/03/03 16:29:35 INFO distcp.DistCp: us3 distcp check & copy succ!!!
```

You can see the task `application_1614496134003_0032` which is the check phase, since no check strategy is specified, only the `does the destination exist` and `is the length equal` strategies are used by default, you can see the statistics indicator `cn.ucloud.us3.fs.distcp.DistCpMapper Two of $US3_DISTCP_CHECK`.

- TOTAL_NUMBER_OF_INPUT_FILES=10: indicates that there are 10 files to be verified.
- NUMBER_OF_DST_NOT_FOUND=10: indicates that none of the 10 files exist at the destination

Then the 10 files will be synchronized automatically and the task `application_1614496134003_0033` will take care of the synchronization, you can see that the final synchronization has been successful.

## Use checksum

The above synchronization is verified, and those that do not pass the verification are automatically synchronized. Here, in addition to the default `destination exists` and `equal length` policies, the example also adds the `checksum check` policy, which uses the `md5` checksum algorithm.

```shell
➜ bin ✗ hadoop jar . /us3-bigdata-adaptor-2.8.5-1.1.0.jar \
cn.ucloud.us3.fs.distcp.
-Dfs.us3.impl=cn.ucloud.us3.fs.US3FileSystem \
-Dfs.us3.access.key=TOKEN_7468973e-d192-4378-8253-xxxxxx \
-Dfs.us3.secret.key=9a632eb8-3938-43a0-a7e1-xxxxxx \
-Dfs.us3.endpoint=internal-cn-bj.ufileos.com \
-Dfs.us3.async.wio.use=false \
-Dfs.us3.async.wio.parallel=2 \
-Dfs.us3.metadata.use=true \
-Dmapreduce.job.user.classpath.first=true \
-Dmapreduce.task.classpath.user.precedence=true \
-libjars . /us3-bigdata-adaptor-2.8.5-1.1.0.jar \
-workspace /workspace/us3distcptest -checksum all -algorithm md5 -enforce /var/log/hadoop-yarn/apps/root/logs/application_1613870792268_ 0067 us3://bigdata-us3/hdfs_backup
21/03/03 16:34:53 INFO distcp.DistCpOptions: set check sum mode:all
21/03/03 16:34:53 INFO distcp.DistCpOptions: set check sum alogrithm:md5
21/03/03 16:34:54 INFO distcp.DistCp: =====================================================================================
21/03/03 16:34:54 INFO distcp.DistCp: The last task has not been processed yet, on workspace:/workspace/us3distcptest
21/03/03 16:34:54 INFO distcp.DistCp: Start processing the last task......
21/03/03 16:34:54 INFO distcp.DistCp: =====================================================================================
21/03/03 16:34:54 INFO distcp.DistCp: us3 distcp need check size/count is 5063546
21/03/03 16:34:54 INFO distcp.DistCp: us3 distcp, drop workspace:/workspace/us3distcptest/checkout
21/03/03 16:34:54 INFO distcp.DistCp: do check job, num of maps:1
21/03/03 16:34:54 INFO distcp.DistCpInputFormat: the size of files count by DistCp, size is 5063546
21/03/03 16:34:54 INFO distcp.DistCpInputFormat: Average size of map: 5063546, Number of maps:1, total size:5063546
21/03/03 16:34:54 INFO mapreduce.JobSubmitter: number of splits:1
21/03/03 16:34:54 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1614496134003_0034
21/03/03 16:34:54 INFO impl.YarnClientImpl: Submitted application application_1614496134003_0034
21/03/03 16:34:54 INFO mapreduce.Job: The url to track the job: http://master2:23188/proxy/application_1614496134003_0034/
21/03/03 16:34:54 INFO mapreduce.Job: Running job: job_1614496134003_0034
21/03/03 16:35:01 INFO mapreduce.Job: Job job job_1614496134003_0034 running in uber mode : false
21/03/03 16:35:01 INFO mapreduce.Job: map 0% reduce 0%
21/03/03 16:35:07 INFO mapreduce.Job: map 100% reduce 0%
21/03/03 16:35:15 INFO mapreduce.Job: map 100% reduce 100%
21/03/03 16:35:15 INFO mapreduce.Job: Job job_1614496134003_0034 completed successfully
21/03/03 16:35:15 INFO mapreduce.Job: Counters: 57
File System Counters
FILE: Number of bytes read=0
FILE: Number of bytes written=348871
FILE: Number of read operations=0
FILE: Number of large read operations=0
FILE: Number of write operations=0
HDFS: Number of bytes read=5065399
HDFS: Number of bytes written=85
HDFS: Number of read operations=27
HDFS: Number of large read operations=0
HDFS: Number of write operations=2
US3: Number of bytes read=0
US3: Number of bytes written=0
US3: Number of read operations=0
US3: Number of large read operations=0
US3: Number of write operations=0
Job Counters
Launched map tasks=1
Launched reduce tasks=1
Other local map tasks=1
Total time spent by all maps in occupied slots (ms)=252819
Total time spent by all reduces in occupied slots (ms)=469334
Total time spent by all map tasks (ms)=4013
Total time spent by all reduce tasks (ms)=3847
Total vcore-milliseconds taken by all map tasks=4013
Total vcore-milliseconds taken by all reduce tasks=3847
Total megabyte-milliseconds taken by all map tasks=8074156
Total megabyte-milliseconds taken by all reduce tasks=15018688
Map-Reduce Framework
Map input records=10
Map output records=0
Map output bytes=0
Map output materialized bytes=6
Input split bytes=116
Combine input records=0
Combine output records=0
Reduce input groups=0
Reduce shuffle bytes=6
Reduce input records=0
Reduce output records=0
Spilled Records=0
Shuffled Maps =1
Failed Shuffles=0
Merged Map outputs=1
GC time elapsed (ms)=170
CPU time spent (ms)=2570
Physical memory (bytes) snapshot=1501753344
Virtual memory (bytes) snapshot=8458620928
Total committed heap usage (bytes)=3024093184
Shuffle Errors
BAD_ID=0
CONNECTION=0
IO_ERROR=0
WRONG_LENGTH=0
WRONG_MAP=0
WRONG_REDUCE=0
cn.ucloud.us3.fs.distcp.DistCpMapper$US3_DISTCP_CHECK
SIZE_OF_DISTINATION_CHECKSUM_READ=5063546
SIZE_OF_SOURCE_CHECKSUM_READ=5063546
TOTAL_NUMBER_OF_INPUT_FILES=10
File Input Format Counters
Bytes Read=1737
File Output Format Counters
Bytes Written=85
21/03/03 16:35:15 INFO distcp.DistCp: us3 distcp check taken: 0 Days 0 Hours 0 Minutes 13 Seconds 9 Milliseconds
21/03/03 16:35:15 WARN distcp.CopyListing: hdfs://Ucluster/workspace/us3distcptest/checkout/_SUCCESS read err:hdfs://Ucluster/workspace/ us3distcptest/checkout/_SUCCESS not a SequenceFile
21/03/03 16:35:15 INFO distcp.DistCp: us3 distcp, all file integrity verification passed, no need to copy ~ ^_^
```

Finally, the verification passed.

## Incremental migration

You need to delete the `workspace` specified directory first, and then follow the same method as before ***use sync***

## Defects

The current drawbacks are the following.

- Synchronized files cannot be accessed in close proximity. Cannot group files from the same node on the hdfs to be synchronized into a specified task and have the task assigned to the corresponding hdfs node.
- does not support regular, wildcard and other filtering rules.
- Can not limit the flow of a single task. But the current synchronous write speed is stable at about 30MiB/s.
- The full amount of checksum checksum will lead to a lot of traffic traversal. Later can support US3 Etag checksum, the traffic is limited to the hdfs cluster inside.
- Task allocation algorithm is not uniform.
