# Frequently Asked Questions

## What is the object storage space and Key?

An object storage space (or storage space for short) is an organizational unit of file management, and a file must be located in a space. The name of the space is globally unique and cannot be modified.

The file name is the name of the corresponding file, which is globally unique in the storage space, and each file name identifies a file in the storage space. Uploading a file with the same file name will cause the original file name file to be overwritten.

## What is the difference between public space and private space?

Public space means that anyone can access the files in that space directly through a URL, without the need for an authorized signature.

Private space requires the correct signature to be generated based on the API public-private key to access the file.

## How to view and manage the uploaded files?

Users can view the uploaded files through the file management page in the console, and they can also view the uploaded files using the file management tool or the API.

## Does it support directory and file list?

Object Storage does not have the concept of directory, so you cannot list files by directory.

However, when uploading files, Key still uses the directory format, which is convenient for specific user scenarios.

For example: demobucket.ufile.ucloud.cn/test/a.jpg where key=test/a.jpg.

## How to use the two domains provided by the object storage space?

Each storage space provides one storage space domain name and one CDN acceleration domain name by default.

File upload operations must send the request to the storage space domain.

File download operations can be performed by accessing either the storage space domain or the CDN-accelerated domain. It is recommended to use a CDN-accelerated domain for file downloads for a better download experience.

## How much data can I store? Is there a capacity limit for the object storage space?

There is no limit to the total amount of data you can store and the number of objects you can use on demand.

## What is the limit on file size?

The maximum size of a single file is 5TB.

## How can the object storage be accessed through the intranet?

1. the API of space management, the domain name of intranet access is the same as public network, use `api.ucloud.cn`.

2. For the file management API, you need to use the intranet-specific domain name `<bucket_name>.ufile.cn-north-02.ucloud.cn`.

For example, if the bucket name is demobucket, then its intranet domain name is `demobucket.ufile.cn-north-02.ucloud.cn`.

The list of APIs for file management is as follows: PutFile, PostFile, UploadHit, GetFile, DeleteFile, InitiateMultipartUpload, UploadPart, FinishMultipartUpload, AbortMultipartUpload. AbortMultipartUpload.

Command line tool, accessed via intranet, you need to change `proxy_host` in the configuration file to `"proxy_host":'www.ufile.cn-north-02. ucloud.cn'`.

For the SDK (phpSDK for example) to access via intranet, you need to change `$UCLOUD\_PROXY\_SUFFIX` to `$UCLOUD\_PROXY\_SUFFIX = 'ufile.cn-north-02 .ucloud.cn'` in the configuration file (other SDKs usually change the configuration file to proxy _suffix).

## How to delete a large number of objects?

You can delete files in the storage bucket by setting [lifecycle](/ufile/guide/lifecycle).

## What should I do if I get a timeout error when using the filemgr tool?

"You can use mput and add `--speedlimit` to limit the speed.

## US3 What should I do if my domain is told by a third party platform that it is a security risk?

The third-party platform's security detection is based on pan-domain detection and blocking, while different US3 customers' domains use the same pan- The third-party platform's security detection is based on pan-domain detection and blocking, while different US3 customers' domains use the same pan-domain, so as long as one customer has illegal content, the whole US3 default domain will be blocked by the security software.

We have communicated with the third-party platform for many times about this issue, but the other party refused to do the blocking according to the cost of Therefore, if you encounter a blocking situation, we suggest you to use a custom domain name to solve it. configuration method, please refer to [Domain Management](/ufile/guide/domain).

## How to apply when I have cross-domain requirement?

If you want to configure cross domain in US3, you need to assign a work order to technical support, and specify the bucket name, US3 domain name, Origin address and http method to be cross domain.

## How is the traffic of CDN back to US3 billed?

CDN back to source, the traffic flows from US3 to UCDN, this part of traffic is not billed by UCDN, but by US3, please refer to the following figure.

! ! ![](/images/UCDN back to the source US3.png)
The billing price is detailed in: [metering billing](/ufile/bill/new)

## Supported space types for mirror back to source

Mirror back to source has no signature process and currently only supports public space.

## Why can't my account perform storage space or file operations?

If your account is prompted with `291:[xxx] This account does not have permission to perform the corresponding Action and product type` in the console operation, it means that the sub-account you are currently using is not authorized to perform the relevant US3 object storage operation permission, Please contact the main account administrator to open the relevant permission.

## How to store additional file metadata information?

US3 API supports users to store custom metadata not more than 8KB, when users call API for file upload request, you can add `X-Ufile-Meta-*` field in request header, such as adding file MD5 information, you can add request header `X-Ufile-Meta-MD5`, when you execute Head, Get request, you can get `X-Ufile-Meta-MD5`. Ufile-Meta-MD5` from Response Header to get the content of `X-Ufile-Meta-MD5:[*]`. For more details, please refer to [Object Storage API Documentation](https://docs.ucloud.cn/api/ufile-api/README)

**Note: The `X-Ufile-Meta-xxx` restriction `xxx` in the Header request can only contain alphabetic characters, numbers and ligatures (short horizontal '-')**

## Enter the file management page after subaccount authorization prompt: illegal authorization

When you add a sub-account, if you don't select "API Access", then the sub-account has no key by default

! [](/images/subaccount_authorization_1.png)

There are two modification entrances

- For accounts with administrative privileges: go to the user management page, go to the details page of a specific user, and then create a key

! [](/images/subaccount_authorization_2.png)

- Create a key for the currently logged-in account: go to the account management page of the current account, click the API key menu, and create the key

! [](/images/sub-account_authorization_3.png)

The first one is suitable for managers, the second one is suitable for sub-accounts to create keys for themselves
