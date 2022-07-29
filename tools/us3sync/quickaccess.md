#Get started quickly
-[get started quickly] (# get started quickly)
-[video teaching] (\video teaching)
-[interface description] (# interface description)
-[login interface description] (# login interface description)
-[description of task management interface] (# description of task management interface)
-[error log export] (# error log export)
-[create task interface description] (\create task interface description)
-[you can get jobid here to build fetch requests] (\
-[create migration source interface description] (\create migration source interface description)
-[create node interface description] (\create node interface description)
-[instructions for use] (# instructions for use)
-[add endpoint] (# add endpoint)
-[create task] (# create task)
-[start migration] (# start migration)
-[pause migration] (# pause migration)
-[retry] (\
-[export failed file list] (\export failed file list)
-[cache description] (# cache description)
-[migration type description] (# migration type description)
- [S3](#s3)
- [local](#local)
- [http](#http)
-[instructions for use] (# instructions for use)
-[add endpoint] (# add endpoint)
-[create task] (# create task)
-[start migration] (# start migration)
-[pause migration] (# pause migration)
-[retry] (\
-[export failed file list] (\export failed file list)
-[cache description] (# cache description)
-[migration type description] (# migration type description)
- [S3](#s3)
- [local](#local)
- [http](#http)
-[instructions for use] (# instructions for use)
-[add endpoint] (# add endpoint)
-[create task] (# create task)
-[start migration] (# start migration)
-[pause migration] (# pause migration)
-[retry] (\
-[export failed file list] (\export failed file list)
-[cache description] (# cache description)
-[migration type description] (# migration type description)
- [S3](#s3)
- [local](#local)
- [http](#http)
-[instructions for use] (# instructions for use)
-[add endpoint] (# add endpoint)
-[create task] (# create task)
-[start migration] (# start migration)
-[pause migration] (# pause migration)
-[retry] (\
-[export failed file list] (\export failed file list)
-[cache description] (# cache description)
-[migration type description] (# migration type description)
- [S3](#s3)
- [local](#local)
- [http](#http)
##Video teaching
Watch the following video to quickly start deploying and using us3sync
<video id="video" length=1000 width=800 controls="" preload="none" poster=" https://static.ucloud.cn/c90df1569fa689282e518df798ce7e9d.png ">
<source id="mp4" src=" http://caozuozhinan.cn-bj.ufileos.com/ Screen recording 4 US3 sync.mp4 ">
</video>
##Interface description
After the service is started, open the page <https://<web service monitoring ip>: < web service monitoring port >, and you can access the us3sync> interface to migrate data on the interface.
###Login interface description
! []( https://ufile-release.cn-bj.ufileos.com/us3sync/doc/login_2.png )
###Description of task management interface
! []( https://ufile-release.cn-bj.ufileos.com/us3sync/doc/detail_2.png )
###Error log export
! []( https://ufile-release.cn-bj.ufileos.com/us3sync/doc/error_log_2.png )
```
-Task operation:
Start migration: start file migration
Start verification: verify the source side and directory side files based on size, and pull the full list of files from the source side and the destination side for comparison
Management - retry: if there is a failure record after the file is migrated, use this button to migrate the failed file again
Manage - Export: if there is a failure record after migrating files, use this button to export the list of failed files to.Us3sync/details/[jobid]/error Log location
Manage delete: deleting a task will clear the task cache records
Manage clone: open the create task interface and fill in the current task information
-The statistical information is as follows:
=================================
Total migration: the total number of files migrated this time
Waiting for migration: number of files in the queue to be migrated
Migrating: number of files being migrated
Migration successful: number of files that have been successfully migrated
Migration failure: number of files that have failed to migrate
=================================
-Error log export:
If there is an error in your migration task, please click the button on the figure to export the error log after the task is accepted. The log will be uploaded to US3 by us3sync, and a URL that can be downloaded will be provided to you on the interface.
You can use WGet to download on the machine where the node is located, and refer to: https://cms-docs.ucloudadmin.com/ufile/tools/us3sync/questions Conduct troubleshooting.
```
###Create task interface description
! []( https://ufile-release.cn-bj.ufileos.com/us3sync/doc/create_job_2.png )
####You can get the jobid here to build the fetch request
! []( https://ufile-release.cn-bj.ufileos.com/us3sync/doc/jobid.png )
###Create migration source interface description
! []( https://ufile-release.cn-bj.ufileos.com/us3sync/doc/create_endpoint_2.png )
###Create node interface description
! []( https://ufile-release.cn-bj.ufileos.com/us3sync/doc/worker_create_2.png )
###
##Instructions for use
###Add endpoint
Add a migration source, refer to [create migration source interface description] (\create migration source interface description), click the create endpoint button, fill in the corresponding information in the dialog box, and click OK.
###Create task
To create a task, refer to [create task interface description] (\create task interface description), click the Create Task button, fill in the corresponding information in the dialog box, and click OK. If you want to create a fetch task, please click the fetch task option on the interface.
In addition, for jobid, please refer to [get task ID] (\
explain:
The migration process will migrate the files under the specified source prefix to the specified destination prefix, for example:
```
1. The source prefix specifies: a/ the destination prefix specifies B/
At this time, all files under source side a/ will be migrated to destination side B /
If the source side has a file a/a Txt after migration, there will be b/a.txt on the directory side
2. The source prefix specifies: a/ the destination prefix specifies an empty path
At this time, all files under the source side a/ will be migrated to the root directory of the destination side
If the source side has a file a/a Txt after migration, there will be a.txt in the root directory of the directory side
```
Description of coverage condition options. Currently, three methods are supported:
```
>Full coverage: all files existing on the source side are forcibly uploaded to US3
>Condition coverage: determine whether to migrate files according to the specified fields
>> conditional coverage currently supports three fields: file type, file timestamp, and file size
>> file type and file size determine whether the corresponding attributes of the source side file and the destination side file are consistent
>> file timestamp by judging that the source side file is newer than the destination side file, the file will be migrated again
>Do not overwrite: it is found that a file with the same name already exists at the destination. Skip the file
```
Periodic task configuration description:
```
>If the migration needs to be performed regularly, configure the migration every few days and when
```
Description of incremental filter conditions: filter the files to be migrated according to the configuration, and support two rules:
```
1. Filter according to the absolute time, and the user specifies the starting time.
2. Filter according to the latest time, and the user specifies to migrate the files in recent hours. If periodic task execution is configured, each time a periodic task is started, it will be calculated according to the time at that time.
```
Regular matching Description:
```
1. If this item is configured, the regular expression will be matched when pulling the source list, and only the hit file can be migrated.
2. This expression will not match the source prefix, so it can be matched with the prefix to the file that hits the regular expression under a prefix.
```
###Start migration
Refer to [description of task management interface] (\task management interface description), and click the start migration button.
###Pause migration
Refer to [description of task management interface] (\task management interface description), and click the pause button.
###Retry
If there is a failure record after migration, you can retry the file that failed to migrate. Refer to [description of task management interface] (# description of task management interface), and click the management - retry button.
###Export failed file list
If there is a failure record after migration, you can export the list of files that failed to migrate. Refer to [description of task management interface] (\task management interface description), and click the management - Export button.
##Cache description
Us3sync uses pika to provide caching services. Mainly cache the following information:
-Task details
-Migration Statistics
-Migration records
##Migration type description
|Migration type | description|
| -------- | -------------------------------------------------------------- |
|S3 | realize migration based on S3 interface, and cloud platforms that support S3 protocol can migrate based on this type|
|OSS | implement OSS cloud storage interface|
|Qiniu | implement qiniu cloud storage interface|
|Youpai | implement youpai cloud storage interface|
|US3 | implement US3 cloud storage interface|
|NAS | realize NAS system or local file migration|
|URL | realize URL list based migration|
### S3
AWS S3 source needs to provide an additional region field of the space to be migrated. Refer to:< https://docs.aws.amazon.com/zh_cn/general/latest/gr/s3.html > 。
For the third party to implement S3 protocol, refer to the corresponding manufacturer's S3 document description.
### local
The local source needs to provide the path of the directory to be migrated
### http
The HTTP source needs to provide the URL address of the list of files to be migrated.
Each line represents a resource to be migrated, and the format of the file list to be migrated is as follows:
>File URL address
>File URL address