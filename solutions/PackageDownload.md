# How to package and download files on US3

## Introduction

This solution is designed to solve the problem of downloading files from Bucket on US3 after packaging them. The packaged files on US3 are packaged into a ZIP archive by the packaged service, which makes it easy to download multiple files in a batch to the local area.

## Prep

* Create a UHost cloud host with linux OS on [Cloud Host Console](https://console.ucloud.cn/uhost/uhost).
* Get a token on [US3 console](https://console.ucloud.cn/ufile/token) with upload and download, and column fetch permissions for the target bucket.

## Fundamentals

The tool works as follows.

1. the tool itself will pull up an HTTP server
2. the user sends a POST request to submit a packaging task, and you get the Key of the zip archive that will eventually be generated in the response to the request
3. the packaging tool downloads the files in US3 locally and packages them according to the information in the request you submitted
4. upload the local zip archive to the specified location in US3
5. you can download the files according to the zip archive key you obtained in the second article

## Operation steps

We assume here that you have created [Cloud Host UHost](https://console.ucloud.cn/uhost/uhost) and got the corresponding token on [US3 Console](https://console.ucloud.cn/ufile/token).

1. Download the packaging tool [Toolkit](https://github.com/ufilesdk-dev/ufile-pack/releases/)

2. Extract the toolkit `unzip US3-PACK.zip`

3. Modify the [server_conf.json](https://github.com/ufilesdk-dev/ufile-pack/blob/main/server_conf.json) configuration file in the tool with the following configuration file.

   ````json
   {
     "log": {
       "LogDir": "logs",
       "LogPrefix": "zip_",
       "LogSuffix": ".log",
       "LogSize": 50,
       "LogLevel": "DEBUG"
     },
     "http": {
       "Ip": "0.0.0.0",
       "Port": 80
     }, // port and ip for the service to listen on
     "us3_config": {
       "public_key": "xxxxxxxxxxxxxxx", //Public key in Token
       "private_key": "xxxxxxxxxxxxxxxxxx" //Private key in Token
     }
   }
   ````



4. Execute `. /US3-PACK` to start the service, you can also use a background process to execute the service `nohup . /US3-PACK &`

5. At this point you can send a POST request to the root url of the service (e.g. http://xxx.xxx.xxx.xxx) with two types of parameters, one for the task of packaging all files under a certain prefix and one for the task of packaging specific files.

   > Note that if you apply for UHost cloud hosting with only intranet IP, then please send the packaged POST request on the same cloud host, or inside the same VPC.

### Specify prefix for packing

   ```json
   {
       "action": "GetUFileZipRequest",
       "prefix": "prefix",
       "bucket_name": "BucketName",
       "file_host": "internal-cn-sh2-01.ufileos.com"
   }
   ```

   where.

   * the action field specifies the request type

   * bucket_name corresponds to the bucket name of the bucket where you want to package the file

   * prefix is the prefix (folder) path of the bucket where you want to package the file

   * file_host is the endpoint you use to access the bucket, please refer to [locale and domain](https://docs.ucloud.cn/ufile/introduction/region)

### Specify the list of files to be packaged

   ```json
   {
       "action": "GetUFileZipByListRequest",
       "file_list": "prefix/key1,prefix/key2,prefix/key3",
       "bucket_name": "BucketName",
       "file_host": "internal-cn-sh2-01.ufileos.com"
   }
   ```

   Where.
   * the file_list field specifies the name of the file to be packed, note that here the file name includes the file prefix but not the bucket name
   * The other fields are the same as above

> You can find the [event.json](https://github.com/ufilesdk-dev/ufile-pack/blob/main/event.json) file in the zip archive and change the contents of the json according to the request format above and the Change the contents of the json according to the request format above and the packaged task you want to submit, and test it on the same cloud host with this command: ```curl -X POST -d@event.json localhost``

After sending the request, you will receive the address of the zip package in the return. The request return format is as follows.

````javascript
{
    "Action": "GetUFileZipByListRequest",
    "prefix": "prefix",
    "RetCode": 0,
    "ErrMsg":"",
    "Key": "output/xxxx-xxxx-xxxxx-xxxx-xxxxxxx.zip"
}
````

where the key field is the object name of the zip package that the tool uploads to US3 after the package request is processed

### Get the package

You can see if the task is completed by looking at the log file in the logs/ folder

Finally, you can download this file using an http client tool, such as

`wget http://bucket.internal-cn-sh2-01.ufileos.com/output/xxxx-xxxx-xxxxx-xxxx-xxxxxxx.zip`

## Performance tests

With 1 core 1G RAM UHost host and intranet data transfer, the time to pack 55 20M files (total size 1.15G) is about 30S.