# v1.8.0
1. Fixed the problem that S3 type endpoint cannot use HTTPS
2. Add the request timeout configuration of endpoint
# v1.7.0
1. Fixed the bug that only the top-level prefix is verified in the verification phase when prefix auto discovery column retrieval is performed
2. When prefix auto discovery is not checked, the empty directory on the source side will be synchronized to the destination side
3. Support the endpoint of qiniu proprietary cloud
# v1.6.1
1. Fixed the bug that endpoint cannot be created on the VPC
2. Fixed a bug that might get stuck when using the prefix auto discovery function
3. Fixed the bug that endpoint in URL list mode may not be created
# v1.6.0
1. Optimized the statistical method of data
2. Fixed the panic caused by null nextmarker in the returned result when using the S3 interface to pull the list
3. Fixed the failure of upload ID caused by the cancellation of segmented upload request during retry
4. Fixed the bug that the same marker is always used in a list
5. Added the function that if the user name and password are not set at startup, the user name and password can be set at the first login interface
# v1.5.0
1. Add a fetch task mode. You can send a post request to the /fetch path of the server to incrementally synchronize files
2. Now you can specify the --sentinel addr parameter at startup to let the node connect to the pika database using sentinel mode
3. Support HTTPS download and pull list functions when using AWS, US3, OSS, qiniu and URL list type endpoints, which can be configured when creating endpoints
4. It supports the function of automatically discovering new public prefixes when using AWS, US3, OSS, and qiniu type endpoint pull lists. In this mode, the pull list will be listed and retrieved concurrently for each prefix
5. It supports the use of the prefix file list interface to pull lists when using US3 type endpoints. This function is usually used in the VPC environment
6. It supports the function of selecting the fragment size of fragment upload according to the return result obtained when initializing the fragment upload task.
7. Now the task of the source 404 will also be considered as a failure
8. Now the worker will not quit immediately after losing the master heartbeat, but will slow down the heartbeat with an exponential backoff strategy. The maximum heartbeat interval is once every 32 seconds. If the heartbeat of the master node is recaptured, the heartbeat interval will be restored
9. Now the worker will abort the uncompleted fragment upload task correctly
10. Now the worker encounters 40450305004 status code when downloading the file, and will retry 5 times in the form of exponential backoff
11. The process of assigning tasks to workers has been changed. Now the tasks obtained by workers can be quickly recovered after worker downtime
12. Optimize the web interface. At present, you can see the progress of file list operation in the task retrieval and audit stages
13. Now you can use regular expressions to filter the source side object key when creating a task, but this object key does not include the bucket name and prefix configured in the task
# v1.4.0
1. Add the synchronous deletion function. At present, you can check this option when creating a task to delete files that do not exist at the source end but exist at the destination end
2. Optimize the web interface, and separate the synchronization, deletion, and ongoing indicators
# v1.3.1
1. Ignore files that do not exist on the source side in the migration
2. Optimize URL list to pull source list file
3. Fix the process panic caused by the do not overwrite option
# v1.3.0
1. Multi DB optimized column retrieval method
2. Optimize local source column fetching
3. Optimize HTTP source column retrieval
4. Repairing OSS filegroup type migration failed
5. Failed to repair the empty file migration
6. Non scheduled tasks support verifying new files
# v1.2.0
1. Cancel the scheduled task and start it immediately
2. Add MD5 verification support and create task assignments
3. Endpoint supports prefix verification for the token of the specified prefix
4. URL list migration, support skipping 404 links
5. Optimize task distribution and improve migration speed
# v1.1.2
1. Repair NAS, URL list type endpoint cannot be created
2. Repair the uarchive type migration, and the MIME type verification failed
# v1.1.1
1. Fix the task blocking problem
2. The repair task cannot be completed normally
3. Start the script to verify the pre installed command
4. Front end support required item verification
# v1.1.0
1. Support migration source management
2. Support regular tasks
3. Support failed file export
4. Support file overwrite options
5. Support speed limit, QPS
# v1.0.0 
1. Support migration sources: S3, OSS, qiniu, youpai, US3, NAS, URL list.
2. Support web management, manage migration tasks and migrate nodes through web.