


# Download files

## Simple Download

Simple Download is a download of an uploaded file (Object) through the GetObject interface of the US3 API, Object downloads are done using HTTP GET requests.

* See [GetFile](https://docs.ucloud.cn/api/ufile-api/get_file) for more information on the API interface for Simple Download.

* Please refer to US3 access for URL generation rules for Object.

* If you need to use a custom domain name to access Object, please refer to Custom Domain Access US3.

## Breakpoint download

US3 provides the function to start downloading from the location specified by Object, so that when downloading a large Object, you can download it in If the download is interrupted, the download can be continued from the last completed location when restarting.

Similar to simple upload, you need to have read access to the Object. The definition of Range can be found in the HTTP RFC. request header, the return message will contain the length of the entire file and the range of this return. means that the entire file is 44 and the range returned is 0-9. If it is not in the range, then the entire file is transferred and the Content-Range is not If it is not in the range, then the entire file is transferred and the Content-Range is not mentioned in the result and the return code is 206.

## View file list

You can list the files (Objects) you have uploaded in the storage (Bucket) via the PrefixFileList interface in the US3 API.

See [PrefixFileList](https://docs.ucloud.cn/api/ufile-api/prefix_file_list) for more information on the API for viewing the file list.

## Deleting files

Deleting a file means deleting a file (Object) uploaded to the storage (Bucket).

US3 allows you to perform the following deletion actions.

US3 allows you to perform the following deletion actions: * Single delete: Specify an Object to be deleted.

* Automatic deletion: If you need to delete a large number of Objects, and the deleted Objects have a certain pattern, such as regularly deleting Objects before certain days, or emptying the entire Bucket, we recommend using lifecycle management to automatically delete Objects. After setting the After setting the lifecycle rules, US3 will automatically delete the expired Objects according to the rules, thus greatly reducing the number of deletion requests you After setting the lifecycle rules, US3 will automatically delete the expired Objects according to the rules, thus greatly reducing the number of deletion requests you send and improving the deletion efficiency.

Note: This feature is in internal testing phase, please contact technical support if you need to use it.

