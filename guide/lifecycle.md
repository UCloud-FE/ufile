

# Life cycle

You can enable the lifecycle function of a storage space (Bucket) to manage the lifecycle of all files or files with a specific prefix in the storage space ( you can configure one or more rules), set lifecycle rules to convert storage types or delete file operations. You can configure one or more rules), set lifecycle rules to convert storage types or delete file operations.

* Delete files: Support setting specified files to be deleted automatically after N days.

* Convert to low-frequency storage: You can set the specified files to be automatically converted to low-frequency storage type after N days.

* You can set the specified file to be automatically converted to archive storage type after N days.

Usage Note: How is the start time of the type conversion and delete file operations calculated? , and the obtained time is added to the midnight of the day after (CST: China Standard Time), so as to get the object operation start time. For example, if an object is uploaded on January 1, 2015 at 10:00 AM CST and set to be deleted after 3 days, then the file will be deleted within the day after January 5, 2015 at 00: 00 CST.
**Note: After setting the lifecycle rule, the lifecycle operation will be executed on the same night and the deletion operation cannot be recovered, so If a file is uploaded by a user 31 days ago, and the user configures the lifecycle rule to delete the file 30 days ago, the US3 service If a file is uploaded by a user 31 days ago, and the user configures the lifecycle rule to delete the file 30 days ago, the US3 service will automatically perform the delete operation on the night the lifecycle rule is set.

## Usage Scenarios

With lifecycle rules, you can manage your stored data more efficiently and save a lot of labor and storage costs. You can periodically convert non-popular data to low-frequency access or archival storage by setting up rules that match specific prefixes, and delete data that no longer needs to be accessed. For example.

1. surveillance video uploads, according to the relevant regulations, the surveillance retention time is generally specified as follows: 7 days for You can set the lifecycle rules, the You can set the lifecycle rules, the uploaded files, will be automatically expired and deleted after reaching the specified number of days according to the upload time after matching the You can set the lifecycle rules, the uploaded files, will be automatically expired and deleted after reaching the specified number of days according to the upload time after matching the rules hit, thus reducing the storage volume and cost.

Electronic documents are required to be kept for a long time up to 10 years according to the relevant regulations, and the number of accesses to Electronic documents are required to be kept for a long time up to 10 years according to the relevant regulations, and the number of accesses to electronic documents older than 1 year is reduced, and electronic documents older than 3 years are usually no longer accessed. You can reduce the unit cost of storage by setting up lifecycle rules, and uploaded documents will be automatically converted to low-frequency, archival storage after reaching a You can reduce the unit cost of storage by setting up lifecycle rules, and uploaded documents will be automatically converted to low-frequency, archival storage after reaching a specified number of days according to the upload time after matching the rule hit.

## Caution

Once the lifecycle function of the storage space is enabled, it will take effect for all files in the storage space that match the rule range (including Once the lifecycle function of the storage space is enabled, it will take effect for all files in the storage space that match the rule range (including files uploaded before the lifecycle function is enabled), and the lifecycle rule will be triggered at 00:00 on the next day.

2. The operation of deleting files is irreversible, please set the lifecycle rules reasonably according to your needs to avoid losing important data.

## Manage Lifecycle

Select the corresponding space, and select the lifecycle button in the right operation.

! [](/images/guide/managing-lifecycle1.png)

Click the Add rule button.

! [](/images/lifecycle2.png)

Add lifecycle rule screen.

! [](/images/guide/add-lifecycle-rules.png)

|Field |Description |
---- |---- |Rule name
|Rule name |Users can set a rule name to record the rules they create. |Rule Scope
|Rule range |Specifies the range of the rule in effect. When the entire storage is selected, the lifecycle rule takes effect for the entire storage;<br> When the specified directory is selected, it only takes effect for the specified directory under that storage.
|Rule content |Select the lifecycle rule to be executed and the trigger time of the rule, the minimum is 7 days, please refer to the notes for details. Rule content

You can use the other buttons under the operation bar to modify and close the rule.

You can use the other buttons under the operation bar to modify and close the rule. [](/images/lifecycle3.png)

After you click the Disable or Delete button, the status change will take effect the next day and will not affect the tasks that have been triggered but not yet completed at 00:00 on that day.

## Remarks

Both low-frequency storage and archive storage types have a minimum storage period. Deleting, modifying, or overwriting files earlier than the Deleting, modifying, or overwriting files earlier than the minimum storage period requires making up the storage fees for the remaining days that have not yet reached the minimum storage period.

How to calculate the trigger time of automatic operation for life cycle rule?US3 Add up the file upload time with the set number of days, and 00:00 (CST: China Standard Time) of the day after the calculated time is the start time of automatic operation. :00 AM CST and set to be deleted after 3 days, then the automatic file deletion operation will be triggered after January 5, 2015 at 00:00 CST.

3. Currently only Beijing, Guangzhou, Shanghai II and Singapore locales support the life cycle feature, other locales will be supported later.
