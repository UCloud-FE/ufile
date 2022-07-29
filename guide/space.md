
# Storage space

The US3 Object Storage service uploads data files as objects to a bucket. To store files using the US3 service, you need to create a bucket first.

## Create a storage space

Click "Create Storage Space" in the action bar on the top left corner of the space management page.

! [](/images/create-storage-bucket1.png)

In the "Create Storage" pop-up page, you can customize the storage locale, space type, business group, storage domain name, and choose whether to authorize data analysis.

! [](/images/create-storage2.png)

|Field |Description |
---- |---- |Select a locale
|Select a locale |You can select a locale as the physical storage location for the file objects in the storage space, and you cannot change the locale after If you need to access the US3 object storage through UHost cloud intranet, you need to select the same locale as your UHost node . |Space Type
<br>Private space: All files must be authorized by the owner's API key to access, and <br>Private space: All files must be authorized by the owner's API key to access, and supports building temporary URLs to provide user access via the key.
|Business Groups |You can select or create a business group to group the storage space by business group for easy access. |StoreSpace
|Storage space domain name |Fill in the domain name address corresponding to the storage space as the external access address, as the storage space domain |Version control
|Version Control |Select whether to enable version control for the storage space. When version control is enabled, overwriting and deletion operations on data will be retained as multiple historical versions, making it easy for users to recover data. <br>*This feature is in internal testing, please contact technical support if you need to use it.
|Data Analysis | Select whether to license USQL for data lake analysis, which will allow you to mine and analyze text data in the space with USQL data Data Analytics

Once you confirm, the storage space is created successfully. After the storage space is successfully created, users can download the client management After the storage space is successfully created, users can download the client management tools from the console SDK and tools page to manage the space and files, and also use the API or SDK to manage the space and files.

## Delete storage space

Select the specified storage space and click "Delete Storage Space" in the right column.

! [](/images/delete_storage_bucket1.png)

If you want to delete multiple storage bins, select multiple storage bins and click "Delete storage bin" in the upper-left column.

! [](/images/delete-batch-storage.png)


## View storage space

In the storage space list page, you can view the basic information of the storage space, such as storage space domain, region, space type, creation time, etc.

! [image](/images/view-storage-space1.png)

|fields |description |
|---- |---- |
|Domain name of the storage space |The address of the domain name corresponding to the storage space as the external access address. |Field
|The physical storage location of the file objects in the storage space. If you need to access the US3 object storage through the UHost intranet, you need to If you need to access the US3 object storage through the UHost intranet, you need to select the same locale as your UHost node.
|Business Group |The name of the business group where the storage space is located.
<br>Private space: All files must be authorized by the owner's API key to be accessed, supporting the construction of temporary URLs. <br>Private space: All files must be authorized by the owner's API key to be accessed, supporting the construction of temporary URLs to provide user access via the key.
|Data Analytics |Whether or not to authorize USQL for data lake analysis, when enabled, you can mine and analyze text data in the space with USQL data The creation time
|Creation Time |The creation time of the storage space.

Click the name of the storage space or click "Details" in the sidebar to enter the storage space details page to view basic information and monitoring information.

## Modify the storage space type

Select the specified storage space, and click Modify space type in the right action bar.

! [image](images/modify_storage_type1.png)

|space type |description
---- |---- |public space |public read-private write, that is, all files
|public space |public read-private write, that is, all files can be accessed directly by URLs that do not carry signature information (including image |private
|private space |private read-write, i.e., all file and list operations must carry signature information.

**Note: Due to policy restrictions, individual users cannot create public space overseas, if you need to use it, please upgrade to certified corporate users. users.