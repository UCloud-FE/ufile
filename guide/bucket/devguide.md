

# Create storage space

Before uploading files (Objects) to US3, you need to use the CreateBucket interface in the US3 API to create a storage space (Bucket) for storing files with Alternatively, use the [US3 console](https://console. ucloud.cn/ufile/ufile) to create a storage space (Bucket) and set the access rights to the storage space.

## Operation method
|Operation mode |description |
|Operation mode |description |--------- |--------------------------------------------------------------------------------------------------------------- |
|Console |Web application, intuitive and easy to use
|management tools |[management tools](ufile/tools/tools/tools_bcket)
|API |[API](https://docs.ucloud.cn/api/ufile-api/README) |SDK |[SDK](https://docs.ucloud.cn/api/ufile-api/README)
|SDK |[SDK](ufile/tools/sdk)

### Usage restrictions

* The total number of storage spaces created by the same user cannot exceed 20.

* The name of each storage space is globally unique, otherwise it will fail to be created.

* The names of the storage spaces need to conform to the naming convention.

* Once a storage space is created successfully, the name and the locale it is in cannot be modified.


## Set space read and write permissions

You can set the access rights (ACL) of the storage space when you create the storage space (Bucket), or you can modify the ACL of the storage space after Creating the Bucket according to your business needs, and this operation can only be performed by the owner of the storage space.

There are two types of access rights for the storage space.

**Private Space:**
Only the owner of the storage space can write to the files in the storage space, and all files can only be accessed with the owner's API key authorization.

**Public Space:**
Only the owner of the storage can write to the files in the storage, and anyone (including anonymous visitors) can read the files in the storage.

