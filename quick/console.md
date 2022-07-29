# Using the US3 Management Console



## Single Locale Space Management

### Create storage space

After logging in to the console, select "Create Storage" on the space management page.

! [](/images/click-create-storage-button-v4.png)

In the "Create Storage Space" pop-up page, you can customize the storage space domain name, select the space type and storage region.

! [](/images/create-storage-v4.png)

    Public space: All files can be accessed directly via URL.
    Private space: All files must be authorized by the owner's API key to be accessed.

After confirming, the storage space is created successfully. After the storage space is successfully created, users can download the client management tool from the console SDK and tool page for space and file management operations, and also use the API or SDK for space and file management operations.

### View storage space information

In the storage space list page, you can view the basic information of the storage space, such as storage space domain name, region, space type, creation time, etc.

! [image](/images/storage space list v4.png)

Click the storage space to enter the details page to view basic information and monitoring.

### File Management

Select the specified storage space, and click the "File Management" button on the right side to enter the file management page.
! [image](/images/click File Management v4.png)

On the file management list page, you can view the basic information of the file, such as file name, file size, MIME-TYPE, update time, etc.
! [image](/images/file-management-list-v4.png)

You can customize the file list by clicking the Settings button in the upper right corner of the file management.

! [image](/images/custom list settings v4.png)
! [image](/images/custom list v4.png)

### Modify space type

Select the specified storage space and click Modify space type in the right action.

! [image](/images/modify space type v4.png)

    Public space: All files can be accessed directly via URL.
    Private space: All files must be authorized by the owner's API key to be accessed.

### Delete storage space

Select the specified storage space, and select "Delete storage space" in the right action.

! [](/images/delete-spacev4.png)

If you want to delete multiple storage spaces, select multiple storage spaces and click the "Delete storage space" button in the upper-left corner.

! [](/images/batch-deletev4.png)

## Globalized space management

### Create storage space

After logging in to the console, select "Create Storage Space" on the Globalized Space Management page.

In the "Create Storage Space" pop-up page, you can customize the storage space domain name, select the space type, storage source locale, and storage replica locale. The source locale and the replica locale cannot be the same.

! [](/images/globalization-creation.png)

**The source locale and the replica locale cannot be the same.

Note: Globalization US3 temporarily does not support setting back to the source rules, custom domain name function operation details refer to \[ Operation Guide\].

## File Management

### Upload files

Select "Upload File" on the file management page, and set the path prefix in the pop-up window.

! [image](/images/upload file v4.png)

### Modify MIME-Type

! [image](/images/modify-mime-type.png)

### Delete the file

! [image](/images/delete-file-v4.png)

### Download the file

! [image](/images/download file v4.png)

### Get the address

! [image](/images/Get Address popup v4.png)
