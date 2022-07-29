#Tool introduction
##Overview
Us3sync is a migration tool that synchronizes data from different sources to US3. By deploying us3sync in the local or virtual machine, you can easily migrate data from the local or other cloud environment to the US3 storage space. Us3sync tool can create tasks in two modes:
** * normal mode: * * the normal synchronization task can synchronize files with a specified prefix or folder on the source side to US3 in batches.
***fetch mode: * * the synchronization task of fetch mode is to synchronize files to the specified location through post request (describing the URL and destination of the file to be synchronized). It can indicate the callback interface after the success and failure of file synchronization. Us3sync will send the corresponding notification after the synchronization task is completed.
###How common tasks work
! []( https://ufile-release.cn-bj.ufileos.com/us3sync/doc/structure.jpg )
Functions of master node and worker node in the figure:
-Master node:
>Single point deployment, responsible for the management of migration tasks. Its main logic is to pull the file list from the source, and then send the files to be migrated to the worker process for migration.
-Worker node:
>Supports node expansion and is responsible for migrating files. Its main logic is to download files from the source and then upload them to the destination.
The master node and the worker node can be deployed on the same machine or on multiple machines. Users can expand the worker node as needed, as described below:
-Deployed on the same machine:
>The master node and the worker node communicate through the * * internal communication listening address * * configured at startup. The user needs to ensure that the path configured to the worker node is a separate path and cannot be repeated with the master path and other worker paths.
-Deployed on different machines:
>The master node and the worker node communicate through the * * internal communication listening address * * configured at startup to ensure that the address is accessible on the worker machine. The user needs to ensure that the path configured to the worker node is a separate path and cannot be repeated with the master path and other worker paths.
###How the fetch task works
The working principle of fetch type tasks is similar to that of ordinary tasks. The difference is that the source of synchronization tasks needs to * * send post request * * to specify, rather than automatically pull according to the configured source location.
####Post request syntax
```HTTP
POST /fetch/ HTTP 1.1
Authorization: Auth
Content-Type:application/json
```
#####Request header
Authorization is required, and content type, date, etc. are optional
For example: authorization: ucloud XXXX XXXX XXXX XXXX XXXX: XXXXXXXXXXXXX
The pseudo code of the calculation method is as follows:
```go
method := "POST"
md5 := xxxxxx
contentType := xxxxxx
date := xxxxx
privateKey = xxxxx-xxxx-xxxxx-xxxx
publicKey = xxxx-xxxx-xxxx-xxxx
strToSign = method + "\n" + md5 + "\n" + contentType + "\n" + date + "\n"
signature = HmacSHA1(strToSign, privateKey)
signature = Base64(signature)
Authorization: "UCloud " + publicKey + ":" + signature
```
#####Request content
|Name | description | type | required|
| ------------------ | ------------------------------ | ------ | ---- |
|URL | source resource address, URL encode | string | yes|
|Key | file path in bucket, no URL encode | string | yes|
|Bucket | bucket name | string | yes|
|Jobid | fetch task ID | string | yes|
|Successcallbackurl | callback address successfully pulled back to the source | string | no|
|Failurecallbackurl | callback address of source pull failure | string | no|
The jobid here can be obtained in the interface
######Return content
|Name | description | type|
| ------- | ------------ | ------ |
|Retcode | request status code | int|
|Errmsg | request information | string|
|Taskid | unique ID of the task | string|
```JSON
{
"RetCode":,
"ErrMsg":,
"TaskId":
}
```
#####Sample request
```Http
POST /fetch/ HTTP/1.1
Authorization:Authorization: UCloud this-is-my-public-key:AAAArandomsignature=
Content-Type:application/json
Content-Length: 159
{
"Url": " http://xxx.xxx.xxx/xxx/movie.mp4 ",
"Bucket": "example_bucket",
"Key":"movie.mp4",
"JobId": "xxxxxxxxxxxxxxxxxxxx",
"SuccessCallbackUrl":" http://xxx.xxx.xxx/xxx ",
"FailureCallbackUrl":" http://xxx.xxx.xxx/xxx "
}
```
#####Back
```http
HTTP/1.1 200 OK
Content-Type: application/json;  charset=utf-8
Content-Length: 122
Connection: keep-alive
{
"RetCode":0,
"ErrMsg":"success",
"TaskId": "d4d62b79-b292-411a-a1f2-47369e2b532f"
}
```
####Callback content
You can specify the callback address after the task is completed in the request, and us3sync will relax the post request to the corresponding address according to the task execution result. The example of the request content is as follows:
##### Failure callback
```JSON
{
"Code":1,
"TaskId": "d4d62b79-b292-411a-a1f2-47369e2b532f",
"Message":"We encountered an internal error.",
"Resource":" http://xxx.xxx.xxx/xxx/movie.mp4 ",
}
```
##### Success callback
```JSON
{	
"Code": 0,
"TaskId": "d4d62b79-b292-411a-a1f2-47369e2b532f",
"ETag":"xxxxxxxx",
"Key":"xxxxxxxxxxxxxxx.mp4",
"SHA1":"0bc51013e87869137a432200f57daf6affdd3d0c",
"Size":638304718
}
```
##Main functions
1. Support the migration of data from S3, OSS, qiniu, youpai, US3 cloud storage to US3.
2. Support data migration from NAS storage or local directory to US3.
3. Support the specified URL resource list to migrate data to US3.
4. Support web management, manage migration tasks and migrate nodes through web.
5. Support source side management
6. Support the regular execution of configuration tasks
7. Support the export of failed file list
**Note: the migration of files with archive type at the source to US3 is not supported temporarily**
##Document structure
```
US3SYNC
├── bin
│ | -- master # master executable
│ └ -- worker \worker executable program
├── conf
│ └── config. Toml # configuration file
ⅶ - cert \https certificate
ⅶ - log \master log file storage path
♪ - pika \rely on Pika
└── console. SH # startup script
```
##Comparison with the original migration tool
For the use of the original migration tool, please refer to: [original migration tool] (/ufile/tools/tools/ufile_import)
| US3SYNC              | ufile-import                       |
| ------------------------ | ---------------------------------- |
|Provide interface management operations | only command line operations are supported|
|The configuration file is consolidated into a single | multiple configuration files|
|Improve migration efficiency without data dropping | when data is dropped, disk resources need to be provided as needed|
|Using pika cache | using redis cache|
|Concurrency by fragmentation granularity, stable bandwidth | concurrency by file granularity, not friendly to large file migration|
|Data verification by size is supported | verification is not supported|
##Version and running environment
###Software version
Current version: 1.4.0
###Operating environment
- Linux：
-CentOS 7.0 and above (can be viewed through 'cat /etc/redhat-release')
-Ubuntu 16.04 and above (can be viewed through 'cat /etc/issue')