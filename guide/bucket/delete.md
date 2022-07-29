

# Delete storage space

You can delete the storage space you have created through the DeleteBucket interface of the US3 API.
Details of the DeleteBucket API can be found in [DeleteBucket](https://docs.ucloud.cn/api/ufile-api/delete_bucket)

** If the storage space is not empty (there are files in the storage space or it is an uncompleted slice upload), the storage space cannot be deleted.

All files and unfinished slice files in the storage space must be deleted before the storage space can be successfully deleted. If you want to delete all files inside the storage space, it is recommended to set [lifecycle](/ufile/guide/lifecycle) for the files inside the storage bucket to be deleted.

## Operation method
|Operation mode |description
|Operation mode |description |--------- |--------------------------------------------------------------------------------------------------------------- |
|Console |Web application, intuitive and easy to use
|management tools |[management tools](ufile/tools/tools/tools_bcket)
|API |[API](https://docs.ucloud.cn/api/ufile-api/README) |SDK |[SDK](https://docs.ucloud.cn/api/ufile-api/README)
|SDK |[SDK](ufile/tools/sdk)
