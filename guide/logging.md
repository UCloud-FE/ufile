

# Log management

When users access US3 files, a large number of access logs will be generated, and you can enable the log management function. The access logs of this space will be stored in the space specified by the user by hour, according to the fixed naming after it is turned on.

## Set log storage

Select the corresponding space, and click the Log Management button in the right operation
Select the corresponding space, and click the Log Management button in the right operation ! [](/images/guide/log-management1-1.png)

Click the button to enable the log storage function
! [](/images/guide/log-management1-2.png)

Select the log storage space and fill in the log prefix, the default log storage space is the current space, the default log prefix is `ufile-log/`, the prefix can be empty.
Example: fill in the prefix `ufile-log/2019/`, the log will be stored in the filled directory after it is generated.

## Log naming rules

```\<TargetPrefix\><SourceBucket\>-YYYY-MM-DD-HH-UniqueString.csv``

Naming rule field description:

|Field|Description|
|Field|Description|----|----|
|TargetPrefix| specified by the user, indicates the name prefix for storing access log records, can be empty|
|YYYY-MM-DD-HH|is the year, month, day, and hour, respectively, of the Arabic numerals when this log was created|
|UniqueString| is a system-generated 8-bit string that uniquely identifies the Log file|

Examples of names for storage space access logs are as follows.
```ufile-log/SourceBucket-2018-10-22-08-00000000.csv``

## Log format and examples

The logs are provided in csv format, with the log header as the first line of the log, corresponding to the meaning of each column in the body of the log below.
The log header is as follows:
```Action,Bucket,Key,Host,UserAgent,RemoteIP,Referer,HttpStatus,TimeStamp,CostTime,ErrCode,ErrMsg,IsUCDN,FileSize,StorageClass, CreatedTime```


### Field Description
|Field|Example|Description|
----|----|----|----|
|Action|LIST_OBJECTS|interface of the request|
|Bucket|my-bucket|the requested bucket|
|Key|example-object|The name of the requested object|
|Host|example-bucket.cn-bj.ufileos.com|The target domain name of the requested access|
|UserAgent|curl/7.77.0|HTTP's User-Agent header|
|RemoteIP|192.168.0.1|The IP of the requester|
|Referer|https://console.ucloud.cn/|HTTP Referer header for the request|
|HttpStatus|200|HTTP status code|
|TimeStamp|1654653684|The timestamp of the request|
|CostTime|13|The time taken for the request (ms)|
|ErrCode|0|Request error code (0 is no error)
|ErrMsg|success|request error message (success is a successful request)|
|IsUCDN|false|whether UCDN is used|
|FileSize|1024|File size(B), 0 for non-GET requests|
|StorageClass|STANDARD|file storage type: STANDARD means standard storage; IA means low frequency access storage; ARCHIVE means archive storage. This value is UNKNOWN for non-GET requests|
|CreatedTime|1654653681|File creation time, the value is 0 for non-GET requests|

## Logging example
``` bash
Action,Bucket,Key,Host,UserAgent,RemoteIP,Referer,HttpStatus,TimeStamp,CostTime,ErrCode,ErrMsg,IsUCDN,FileSize,StorageClass, CreatedTime
LIST_OBJECTS,example-bucket,,example-bucket.cn-bj.ufileos.com,Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML like Gecko) Chrome/102.0.5005.61 Safari/537.36,10.75.220.2,https://console.ucloud.cn/,200,1654653684,13,0,success,false,0,UNKNOWN,0
OPTIONS,example-bucket,,example-bucket.cn-bj.ufileos.com,Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML like Gecko) Chrome/102.0.5005.61 Safari/537.36,10.75.220.2,https://console.ucloud.cn/,200,1654653684,0,0,success,false,0,UNKNOWN,0
OPTIONS,example-bucket,,example-bucket.cn-bj.ufileos.com,Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML like Gecko) Chrome/102.0.5005.61 Safari/537.36,10.75.220.2,https://console.ucloud.cn/,200,1654653681,0,0,success,false,0,UNKNOWN,0
```
