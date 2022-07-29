

# Features Overview

## Storage Functions

US3 products provide support for the following common upload and download functions for object storage.

### Common upload (Put File)

A normal upload is a single File uploaded by the user using the Put File method in the Object Storage API, which can be used in any HTTP request interaction to A normal upload is a single File uploaded by the user using the Put File method in the Object Storage API, which can be used in any HTTP request interaction to complete the upload scenario, such as the upload of small files.

### [Form Upload (Post File)](https://docs.ucloud.cn/ufile/guide/file/put?id=%e8%a1%a8%e5%8d%95%e4%b8%8a%e4%bc%a0)

Form upload is an upload method for small file uploads, where the user uses the Post File request in the Object Storage API to complete the File upload.

### [Split Upload (Multipart Upload)](https://docs.ucloud.cn/ufile/guide/file/put?id=%e5%88%86%e7%89%87%e4%b8%8a%e4%bc%a0)

Multipart Upload is a way to upload a file by dividing the file to be uploaded into multiple chunks, and then calling the Object Storage API to combine these Multipart Upload is a way to upload a file by dividing the file to be uploaded into multiple chunks, and then calling the Object Storage API to combine these parts into a File after the upload is completed.

### UploadHit

If the file already exists in the object storage, the upload can be completed without uploading the file; otherwise, we need to call other upload interfaces to upload the file.

### Common download (Get File)

Normal download refers to the user downloading the file that has already been uploaded, and the file download is done by using the GET request of HTTP.

### Fragmented download (Range Get)

Split download is a function that starts downloading from the location specified by File, and can be divided into multiple downloads for large files. the download is interrupted, the download can be resumed from the last completed location when restarting.

## Management function

US3 products provide rich management functions in addition to supporting standard object storage upload and download functions.

### [Domain Management](https://docs.ucloud.cn/ufile/guide/domain)

The object storage space supports providing test domain names for access, and you can also choose to bind a custom domain name to create CDN acceleration.

### [Static Web Hosting](https://docs.ucloud.cn/ufile/guide/static_websit_hosring)

Static web hosting policies can be configured via the console for US3 storage buckets that have been bound to a custom domain.

### [Lifecycle](https://docs.ucloud.cn/ufile/guide/lifecycle)

Enabling the storage lifecycle feature allows you to implement lifecycle management for all files or specific prefix files in the storage, set lifecycle rules for archival storage or delete file operations.

### [Mirror back to source](https://docs.ucloud.cn/ufile/guide/mirror)

When the data you request from US3 storage space does not exist, you can set the mirror back to source rule to read back the data in various ways to meet your needs for data hot migration and specific request redirection.

### [Cross domain access](https://docs.ucloud.cn/ufile/guide/cors)

Cross-domain access (CORS) is an operation when a user goes from a web page in one domain to request resources from another domain. You can restrict the permission of cross-domain access through the cross-domain access policy management function.

### [Cross-domain replication](https://docs.ucloud.cn/ufile/guide/multisite)

You can select two different geographic storage spaces to bind for file synchronization. It provides storage space cross-territory disaster recovery capability while being able to meet your data replication needs.

### [Log Management](https://docs.ucloud.cn/ufile/guide/logging)

After setting up the log management storage, all users' access logs to the storage will be stored in the storage you specify by hour, according to the fixed naming.

### [Token Management](https://docs.ucloud.cn/ufile/guide/token)

The token function allows you to open storage space and file management privileges flexibly according to users' needs.

## Value-added functions

### [Image processing](https://docs.ucloud.cn/ufile/service/pic)

Supports a series of image processing operations for images uploaded and saved in the object storage.

### [Decompression service](https://docs.ucloud.cn/ufile/service/zip)

Decompression service is a low-cost and highly reliable decompression service provided by UCloud, which can set rules to automatically decompress the uploaded compressed packages.

### [Document Preview](https://docs.ucloud.cn/ufile/service/doc_preview)

The document preview function supports online preview of text files, presentation files, table files, and pdf files stored in US3.


