

# Space management tools

Space management tool helps users to get storage related information and manage space. It supports creating storage space, deleting storage space, viewing and modifying storage space information and querying storage space file list.

### Download link

[bucketmgr-win32](http://tools.cn-bj.ufileos.com/bucketmgr-win32.zip)

[bucketmgr-win64](http://tools.cn-bj.ufileos.com/bucketmgr-win64.zip)

[bucketmgr-linux64](http://tools.cn-bj.ufileos.com/bucketmgr-linux64.tar.gz)

[bucketmgr-mac](http://tools.cn-bj.ufileos.com/bucketmgr-mac.tar.gz)

### Usage Notes

Linux/Mac users please execute in terminal, Windows users please execute in cmd terminal. Before using, please modify the configuration file config.cfg in the current directory to add the API key to the configuration item: ###

```
{
   "public_key" : "paste your public key here",
   "private_key" : "paste your private key here",
   "api_host" : "api.spark.ucloud.cn"
}
```

Note: The API key can be obtained from the "API Key" page in the console. Put the public\_key and private\_key into the corresponding location in config.cfg file, and the client tool will use them to complete the authentication. Please keep the API key properly to avoid leakage.

#### Create storage space

It is recommended to use the console to create the storage space.

```
. /bucketmgr --action CreateBucket --bucket bucketname --type public --region region --project-id project-id
Parameter description:
--bucket: the name of the storage space to be created
--type: the type of space to be created, public or private
--region: the region of the repository to be created, default Beijing
--project-id: the project group ID of the space to be created
```

Example.

Create uclouddemo storage space under org-mutvtj project.

```
. /bucketmgr --action CreateBucket --bucket uclouddemo --type public --region cn-bj --project-id org-mutvtj
```

#### Delete storage space

Deleting a storage space is recommended using the console.

```
. /bucketmgr --action DeleteBucket --bucket bucketname --project-id project-id
Parameter Description:
--bucket: the name of the storage space to be deleted
--project-id: The project group ID of the space to be deleted
```

#### Get space information

```
. /bucketmgr --action DescribeBucket --project-id project-id
Parameter description:
--bucket: the name of the storage space to be queried
--project-id: The project group ID of the space to be retrieved
```

#### Get the list of files

```
. /bucketmgr --action GetFileList --bucket bucketname --limit limit --pattern pattern --format format
Parameter description:
--bucket: The name of the storage space to be pulled for the list
--limit: the number of records to be queried (default is all)
--pattern: the template of the records to be queried, supports POSIX regular expressions
--format: format the query records
```
