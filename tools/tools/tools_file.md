# File management tools
File management tools help users to manage file operations in the storage space, including.
1、Uploading single file: normal upload (PUT), second upload (UPLOAD-HIT), piecewise upload (MPUT), form-based upload.
2、Uploading folder: normal upload (PUT), piecewise upload (MPUT), incremental upload (SYNC).
3、Download files/folders: normal download (DOWNLOAD), slice download (MDOWNLOAD), batch download (BATCH-DOWNLOAD)
4、Deleting files/folders: general deletion (DELETE), batch deletion (BATCH-DELETE)
5, calculate etag (ETAG)
6, check whether the file exists (CHECK)
7, get the file url (FETCHURL)
8, pull the list of files (GETFILELIST)
### Download link
[filemgr-win32]( http://tools.ufile.ucloud.com.cn/filemgr-win32.zip )
[filemgr-win64]( http://tools.ufile.ucloud.com.cn/filemgr-win64.zip )
[filemgr-linux64]( http://tools.ufile.ucloud.com.cn/filemgr-linux64.tar.gz )
[filemgr-mac]( http://tools.ufile.ucloud.com.cn/filemgr-mac.tar.gz )
The file size of normal upload (PUT), form upload method, and second upload file (UPLOAD-HIT) cannot exceed
To upload files larger than 1GB in size, you must use the MPUT method.
### Instructions for use
This tool is used to do file-specific operations such as upload/download/delete from the command line.  If you need to upload files dynamically, please use the API for content management operations.
For uploading files over 100MB in size, poor network conditions, and frequent disconnections with US3 servers, please use the MPUT in the tool.
Before using it, please modify the configuration file config. cfg in the current directory and add the API key to the configuration item: ```.
```
{
"public_key" : "paste your public key here",
"private_key" : "paste your private key here",
"proxy_host" : "www.cn-bj.ufileos.com",
"api_host" : "api.spark.ucloud.cn"
}
```
The proxy\_ host is different for different locales, as follows: 
```
Beijing
Extranet: <bucket_ name>.cn-bj.ufileos. com 
Intranet: <bucket_ name>.ufile.cn-north-02.ucloud.cn
Shanghai II
Extranet: <bucket_ name>.cn-sh2.ufileos.com
Intranet: <bucket_ name>.internal-cn-sh2-01.ufileos.com
Hong Kong
Extranet: <bucket_ name>.hk.ufileos.com
Intranet: <bucket_ name>.internal-hk-01.ufileos.com
Guangdong
Extranet: <bucket_ name>.cn-gd.ufileos.com
Intranet: <bucket_ name>.internal-cn-gd-02.ufileos.com
Los Angeles
Extranet: <bucket_ name>.us-ca.ufileos.com
Intranet: <bucket_ name>.internal-us-ca-01.ufileos.com
Jakarta
Extranet: <bucket_ name>.idn-jakarta.ufileos.com
Intranet: <bucket_ name>.internal-idn-jakarta-01.ufileos.com
Singapore
Extranet: <bucket_ name>.sg.ufileos.com
Intranet: <bucket_ name>.internal-sg-01.ufileos.com
Seoul
Extranet: <bucket_ name>.kr-seoul.ufileos.com
Intranet: <bucket_ name>.internal-kr-seoul.ufileos.com
Nigeria
Extranet: <bucket_ name>.afr-nigeria.ufileos.com
Intranet: <bucket_ name>.internal-afr-nigeria.ufileos.com
Taipei
Extranet: <bucket_ name>.tw-tp.ufileos.com
Intranet: <bucket_ name>.internal-tw-tp.ufileos.com
São Paulo
Extranet: <bucket_ name>.bra-saopaulo.ufileos. com 
Intranet: <bucket_ name>.internal-bra-saopaulo.ufileos.com
Dubai
Extranet: <bucket_ name>.uae-dubai.ufileos.com
Intranet: <bucket_ name>.internal-uae-dubai.ufileos.com
Vietnam
Extranet: <bucket_ name>.vn-sng.ufileos.com
Intranet: <bucket_ name>.internal-vn-sng.ufileos.com
Mumbai
Extranet: <bucket_ name>.ind-mumbai.ufileos.com
Intranet: <bucket_ name>.internal-ind-mumbai.ufileos.com
Washington, D.C.
Extranet: <bucket_ name>.us-ws.ufileos.com
Intranet: <bucket_ name>.internal-us-ws.ufileos.com
Frankfurt
Extranet: <bucket_ name>.ge-fra.ufileos.com
Intranet: <bucket_ name>.internal-ge-fra.ufileos.com
````
The API key can be obtained from the console in [API Product - API Key]( https://console.ucloud.cn/uapi/apikey ), click Show API Key.  Put the public\_ key and private\_ key into the corresponding location of config. cfg file respectively, and the client tool will use this key to complete the authentication.  Please keep the API key in a safe place to avoid leakage.
For the command line tool, please execute it in terminal for Linux/Mac users and in cmd terminal for Windows users. 
The following demo uses Linux64 platform command filemgr-linux64, Windows-64 users please replace it with filemgr-win64, Windows-32 users please replace it with filemgr-win32, Mac users please replace it with filemgr-mac.
**Note: When the tool is executed in the background, please add the parameter `--nobar=true`. ** 
#### slice upload single file
Please use split upload when the file is large.  Split upload allows renewal in case of failure of a split, and multiple splits can be uploaded concurrently, suitable for larger file scenarios.
```
. /filemgr-linux64 --action mput --bucket demobucket --key key --file filename [--threads threads] [--retrycount retrycount] [--speedlimit speedlimit]
Parameter description:
--bucket: the name of the bucket to upload to         
--key: the name of the file to be uploaded to the bucket       
--file: the path of the local file to be uploaded
--threads: number of concurrent uploads, default is 5
--retrycount: number of failed retry attempts, default is 10, it is recommended to configure a larger number for large file uploads
--speedlimit: upload speed limit, in bytes/s 
--storageclass: file storage type, standard, low frequency, archive, corresponding valid values: STANDARD, IA, ARCHIVE;  default file type, inherited from Bucket default type.  (Bucket default type is STANDARD)
```
Example.
Upload a local file hello. avi to a storage named demobucket and named world. avi as a split upload.
```
. /filemgr-linux64 --action mput --bucket demobucket --key world. avi --file /opt/video/hello. avi --threads 10 --retrycount 20 --speedlimit 1024
```
**Remarks: filemgr supports auto-renewal for files that failed to be uploaded in slices, just re-execute the command when the execution fails.  Only a single process can be run, multiple processes will cause upload failure.  For concurrent uploads, please select directory upload or incremental upload. **
#### Split upload folder
For folders with more large files in them sliced upload folders are faster than normal upload folders.
```
. /filemgr-linux64 --action mput --bucket demobucket --dir localdir [--threads threads] [--trimpath trimpath] [--prefix prefix] [--retrycount retrycount] [--speedlimit speedlimit]       
Parameter description:           
--bucket: the name of the bucket to upload to         
--dir: the local folder to be uploaded
--threads: number of concurrent uploads, default is 5
--trimpath: truncate the absolute path of the partial path
--prefix: the prefix used to generate the Key of the file, the generated Key is prefix+base(filename) when this parameter is specified
--retrycount: number of failed retry attempts for split uploads, default 10, it is recommended to configure larger uploads for large files
--speedlimit: upload speed limit, in bytes/s
--storageclass: file storage type, standard, low frequency, archive, corresponding valid values: STANDARD, IA, ARCHIVE;  default file type, inherited from Bucket default type.  (Bucket default type is STANDARD)
```
**Remarks: Only a single process can be run, multiple processes will cause the upload to fail. **
#### Normal upload of a single file (for files larger than 4M please use the above split upload)
```
. /filemgr-linux64 --action put --bucket bucketname --key key --file filename [--retrycount retrycount] [--speedlimit speedlimit]
Parameter Description:
--bucket: the storage to be uploaded to
--key: the name of the file to be uploaded to the storage
--file: the path of the local file to be uploaded
--retrycount: number of failed retry attempts, default is 10, it is recommended to configure a larger number for large file uploads
--speedlimit: upload speed limit, in bytes/s 
--storageclass: file storage type, standard, low frequency, archive, corresponding valid values: STANDARD, IA, ARCHIVE;  default file type, inherited from Bucket default type.  (Bucket default type is STANDARD)
```
Example.
Upload a local file ucloud. jpg to a bucket with the name uclouddemo and name it logo. jpg : ``
```
. /filemgr-linux64 --action put --bucket uclouddemo --key logo. jpg --file /home/yours/pictures/ucloud.jpg
```
#### general upload folder
```
. /filemgr-linux64 --action put --bucket bucketname --dir dirname [--trimpath trimpath] [--prefix prefix] [--retrycount retrycount] [--speedlimit speedlimit]
Parameter description:
--bucket: the name of the storage space to upload to
--dir: the local folder to be uploaded
--trimpath: truncate the prefix of the absolute path
--prefix: the prefix to be used when generating the Key of the file, when this parameter is specified the generated Key is prefix+base (filename)
--retrycount: number of failed retry attempts for split uploads, default 10, it is recommended to configure larger uploads for large files
--speedlimit: upload speed limit, in bytes/s
--storageclass: file storage type, standard, low frequency, archive, corresponding valid values: STANDARD, IA, ARCHIVE;  default file type, inherited from Bucket default type.  (Bucket default type is STANDARD)
```
Files uploaded by the folder method will use the absolute path where the files in the folder are located to name the Key by default, if you want to specify a special prefix please use the --prefix parameter.
Example 1: Upload all the files in the $HOME/files folder to a storage called demobucket, and use demo/ as the key name.
as a prefix
If there is a file named 1.jpg in the folder
and the space attribute is public, the file will be accessible via an unsigned url (e.g.,  http://demobucket.ufile.ucloud.com.cn/demo/1.jpg ) after the upload is complete
```
. /filemgr-linux64 --action put --dir ~/files --bucket demobucket --prefix demo/
```
If you do not want to use an absolute path for the Key, you can use --trimpath to truncate part of the pathname.  As follows.
```
. /filemgr-linux64 --action put --dir ~/files --bucket demobucket --prefix demo/ --trimpath /root/test
```
Example 2:
Supp