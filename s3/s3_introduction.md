# AWS S3 Protocol Support Description
## Overview
The S3 protocol was introduced by AWS and has become the de facto standard in the object storage industry.  US3 products add compatibility support for the S3 v4 protocol standard to its own standard.
<video id="video" length=1000 width=800 controls="" preload="none" poster=" https://static.ucloud.cn/d95327bb0a274715ba83f05ac3cda8fb.png  ">
<source id="mp4" src=" http://caozuozhinan.cn-bj.ufileos.com/ Zhou Gongyuan 1 US3 support for S3 protocol.mp4 ">
</video>
### Supported APIs
US3's current S3 protocol module supports the following table for the standard S3 protocol.
| **Number** | **API Name** | **Remarks Description** |
| :------: | ------------------------- | ------------------------------------------------------------ |
| 1 | GET Bucket service | Get the Bucket list, only the Bucket created by the public and private key or Token owner |
| 2 | GET Bucket location | Returns the location domain, not recommended to rely on this API
| 3 | GET Bucket acl | Not much sense, mainly for S3 Browser support, Permission field is always "FULL_CONTROL" |
| 4 | GET Bucket versioning | doesn't make much sense, it's mainly implemented to support S3 Browser, the Status field will always be an empty string |
| 5 | GET Object acl | Not much point, mainly to support S3 Browser, the Permission field is always "FULL_CONTROL" |
| 6 | HEAD Object | Refer to [US3 Compatible S3 API - v2.2.pdf]( http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20 -%20v2.2.pdf) | PUT Object
| 7 | PUT Object | Reference [US3 Compatible S3 API - v2.2.pdf]( http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20 -%20v2.2.pdf) |
| 8 | POST Object | Reference [US3 Compatible S3 API - v2.2.pdf]( http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20 -%20v2.2.pdf) |POST Object
| 9 | PUT Object copy | Reference [US3 Compatible S3 API - v2.2.pdf]( http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20 -%20v2.2. pdf)
| 10 | GET Object | Reference [US3 Compatible S3 API - v2.2.pdf]( http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20 -%20v2.2.pdf) | 11 | List Objects
| 11 | List Objects | Reference [US3 Compatible S3 API - v2.2.pdf]( http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20 -%20v2.2. pdf) pdf) |
| 12 | DELETE Object | Reference [US3 Compatible S3 API - v2.2.pdf]( http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20 -%20v2.2. pdf) pdf) | Delete Multiple Objects
| 13 | Delete Multiple Objects | Reference [US3 Compatible S3 API - v2.2.pdf]( http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20 - %20v2.2.pdf) |
| 14 | Initiate Multipart Upload | Reference [US3 Compatible S3 API - v2.2.pdf]( http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%  20-%20v2.2.pdf) |
| 15 | Upload Part | Refer to [US3 Compatible S3 API - v2.2.pdf]( http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20 -%20v2.2.pdf ) , **Note that only 8MB size partitions are currently supported !!!! ** |  16 | Complete Multipart Upload
| 16 | Complete Multipart Upload | Refer to [US3 Compatible S3 API - v2.2.pdf]( http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%  ) 20-%20v2.2.pdf) |
| 17 | Abort Multipart Upload | Reference [US3 Compatible S3 API - v2.2.pdf]( http://ufile-release.cn-bj.ufileos.com/s3%2FUS3%20%E5%85%BC%E5%AE%B9S3%20API%20 -% 20v2.2.pdf) |
Note:
* PUT Object currently only supports 1GB files, if you need to upload files larger than 1GB, please use the split upload API.
* POST Object currently only supports uploads of files up to 32MB in size.
* US3 S3 currently does not support multi-version functionality.
* ~~ currently US3 S3 does not support user-defined metadata features such as Header header specifying x-amz-meta-*~~.
* ~~ currently US3 S3 does not support storage type, the default is standard type ~~, storage type conversion rules refer to [storage type conversion rules]; * ~~  currently US3 S3 does not support storage type
* ~~ currently US3 S3 does not support signatures with x-amz-content-sha256 as `UNSIGNED-PAYLOAD`~~.
* The MD5 checksum of S3 API is not currently supported (the reason is different from the ETag calculation of S3), it is recommended to turn it off:
For example, AWS S3 Java SDK:
System.setProperty(SkipMd5CheckStrategy.DISABLEGETOBJECTMD5VALIDATION_PROPERTY,"");
System.setProperty(SkipMd5CheckStrategy.DISABLEPUTOBJECTMD5VALIDATION_PROPERTY,"");
### Only signature V4 is supported
Scenarios that support V4 signatures.
1. the URL carries parameters (the x-amz-credential field in the URL Query section).
2. POST (x-amz-credential field in the form);
3. carry parameters in the Header (Authorization field);
### S3's AccessKeyID and SecretAccessKey description
The AccessKeyID (or AccessKey) and SecretAccessKey (or SecretKey) of S3 correspond to the API public and private keys of UCloud, or the Token public and Token private keys provided by the US3 service.
**Note: The bucket that requires either the API public-private key or the Token public-private key to operate, must meet the following conditions:**
* The account that created the bucket and the owner of the API public-private key must be the same.
* The account that created the bucket and the account that created the Token must be the same.
### API supports path style and virtual host style
** The path style format is: `http://\${Endpoint}/\${bucket name}/\${key name}`, the bucket name is used as part of the path. **
For example, if the AWS S3 Java SDK goes off-grid in the UCloud Beijing locale using the US3 S3 service, the settings are as follows.
"AWSCredential credentials = new BasicAWSCredentials(ACCESS_KEY,
SECRET_ KEY);
ClientConfiguration clientConfig = new ClientConfiguration();
...
S3ClientOptions clientOptions = S3ClientOptions.builder().build();
clientOptions. setPathStyleAccess(true); //  indicate that the path style API is used
AmazonS3 conn = new AmazonS3Client(credential, clientConfig);
conn.setS3ClientOptions(clientOptions);
conn.setEndpoint("s3-cn-bj.ufileos.com"); "
**Webhosting style: http://${bucket name}. $ {Endpoint}/${key name}, similar to the URL form currently used by US3. **
### Access to the domain (Endpoint)
| **number** | **geography** | **extranetEndpoint** | **intranetEndpoint** |
| :------- | :------- | :-------------------------- | :----------------------------------- |
| 1 | North China I | s3-cn-bj.ufileos. com | internal.s3-cn-bj.ufileos. com |
| 2 | Shanghai | s3-cn-sh2.ufileos. com | internal.s3-cn-sh2.ufileos. com |
| 3 | Guangzhou | s3-cn-gd.ufileos. com | internal.s3-cn-gd.ufileos. com |
| 4 | Hong Kong | s3-hk.ufileos. com | internal.s3-hk.ufileos. com |
| 5 | Los Angeles | s3-us-ca.ufileos. com | internal.s3-us-ca.ufileos. com |
| 6 | Singapore | s3-sg.ufileos. com | internal.s3-sg.ufileos. com |
| 7 | Jakarta | s3-idn-jakarta.ufileos. com | internal.s3-idn-jakarta.ufileos. com |
| 8 | Taipei | s3-tw-tp.ufileos. com | internal.s3-tw-tp.ufileos. com |
| 9 | Lagos | s3-afr-nigeria.ufileos. com | internal.s3-afr-nigeria.ufileos. com |
| 10 | São Paulo | s3-bra-saopaulo.ufileos. com | internal.s3-bra-saopaulo.ufileos. com |
| 11 | Dubai | s3-uae-dubai.ufileos. com | internal.s3-uae-dubai.ufileos. com |
| 12 | Frankfurt | s3-ge-fra.ufileos. com | internal.s3-ge-fra.ufileos. com |
| 13 | Ho Chi Minh City | s3-vn-sng.ufileos. com | internal.s3-vn-sng.ufileos. com |
| 14 | Washington, DC | s3-us-ws.ufileos. com | internal.s3-us-ws.ufileos. com |
| 15 | Mumbai | s3-ind-mumbai.ufileos. com | internal.s3-ind-mumbai.ufileos. com |
| 16 | Seoul | s3-kr-seoul.ufileos. com | internal.s3-kr-seoul.ufileos. com |
| 17 | Tokyo | s3-jpn-tky.ufileos. com | internal.s3-jpn-tky.ufileos. com |
| 18 | Bangkok | s3-th-bkk.ufileos. com | internal.s3-th-bkk.ufileos. com |
Note: * Currently North China I, Hong Kong, Ho Chi Minh, Seoul, São Paulo, Los Angeles, Washington region already support https protocol, other regions can support path style https, subsequent support for virtual host style https (all regions intranet does not support https)*
### Callback extension support
| **Request Form API Name** | **PUT Object** | **POST Object** | **Complete Multipart Upload** |
| ---------------------------------------------- | -------------- | --------------- | ----------------------------- |
| **Carry Parameters in URL** | √ |  × |  √ |
√:Support
×: Not supported
### Image manipulation support
Refer to [Picture manipulation service](/ufile/service/pic)
### Storage type conversion rules
| **US3 storage type** | **S3 storage type** | **US3 corresponds to S3 default storage type** |
| --------------- | --------------------------------------------------------- | ------------------------- |
| STANDARD | STANDARD<br/>STANDARD_ IA | STANDARD |
| IA | ONEZONE_ IA<br/>INTELLIGENT_ TIERING<br/>REDUCED_ REDUNDANCY | ONEZONE_ IA |
| ARCHIVE | GLACIER<br/>DEEP_ ARCHIVE | GLACIER |