# Decompression services

Decompression service is a low-cost and highly reliable decompression service provided by UCloud. Users can set decompression rules, trigger prefix, target bucket and target path after decompression by setting the storage bucket in the console to realize automatic decompression of *.zip files uploaded under the rule prefix.


## Caution

- At present, decompression is in public beta, only support Shanghai region, only support the decompression of the zip package file suffix
- Each bucket can only create a maximum of ten unzip rules
- Each rule's filtering prefix cannot contain each other, for example, if a rule's filtering prefix is /a/b/c/d, then you cannot create /a/b/c and /a/b/c/d/e prefixes.
- The target directory is not filled by default, it will be automatically decompressed to the prefix of the key you uploaded as the directory
- The permissions of the source bucket token should have upload and download permissions, if the token is set to prefix, it cannot have a conflicting relationship with the filtering prefix of its own rules, otherwise it will fail to decompress
- The permission of the target bucket token should have upload and download permission, if the token is set to prefix, it cannot have conflict with the target directory, otherwise it will fail to decompress.
- Pay attention to the setting of target directory and the setting of filtering prefix, if there are still zip files after decompression, it may also trigger the decompression behavior.
- The maximum decompression time for a single zip package is 10 minutes, any task not completed in more than 10 minutes will fail to decompress
- The creation rule contains two entries, one is the creation rule of bucket file management of storage bucket, and the other is the creation rule under data processing
- There is no charge for decompression at the moment, but it will be charged according to the number of uses, resource usage and public network traffic.

## Configure decompression

### Create rules

- 1. [Log in to the console](https://console.ucloud.cn/ufile/ufile)
- 2. Click to open the target bucket function details, click [Data processing] or [File management]
- 3. Click [Add Rule] in the zip package decompression list
- 4. Fill in the relevant parameters and confirm the creation of the rule
! [image](/images/zip/create rule.png)
! [image](/images/zip/rules.png)

### Introduction of parameters


| parameters | mandatory | description |
|:---------------:|:---------------------------:|:------------------------------|
| trigger prefix | yes | set the trigger prefix for decompression, the *.zip files uploaded in the prefix directory will automatically trigger decompression, the prefixes of the rules cannot exist to contain each other, if there are already rules, you cannot select all files as trigger conditions, if there are already rules that trigger conditions are all files, you cannot create other rules, the directory format a/b/ |
| source bucket token | yes | bucket token needs to have upload and download permissions, if the token permissions set prefix, the prefix should contain the trigger prefix, otherwise it will fail to decompress |
| target bucket | yes | Select the bucket to upload the files after decompression|
| Yes | The bucket token needs to have upload and download permission, if the token permission is set to prefix, then the prefix should contain the target directory, otherwise the decompression will fail |
| Destination path | No | The target path for uploading after decompression, not uploading to the key directory of the uploaded *.zip by default, uploading to the root directory by typing "/", other directory format "a/b/c" | Yes
| unzip configuration | yes | whether the decompressed directory retains the file name as the last level of the directory, the target directory is a/, the compressed file name is b.zip, if you choose to retain, then the decompressed file path is a/b/, if you choose not to retain, then the decompressed file path is a/ |
| rule name | yes | decompression rule name |


### Configuration use case:

- Upload to src prefix, decompress to dst directory, do not keep the compressed file name directory, prefix set to src/, target directory set to dst/
- Decompressed file structure
```
bucket-src
└─── src/
     ├─── a.zip
     └─── b.zip
bucket-dst
└── dst/
     ├── a.txt
     ├─── b.txt
     └─── ...
ðŸ™' ðŸ™'

- Upload to src prefix, decompress to dst directory, keep the compressed file name as directory , prefix set to src/, target directory set to dst/
- The uncompressed file structure
```
bucket-src
└─── src/
     ├─── a.zip
     └─── b.zip
bucket-dst
└── dst/
      ├óΓé¼╦£────a/
      | ──── a.txt
      ├óΓé¼╦£────b/
           |──── b.txt
ðŸ™' ðŸ™'
- Upload to src prefix, decompress to dst directory, keep the zip file named directory ,prefix set to src/, target directory set to empty, default decompression to the key directory of the uploaded *.zip file
- File structure after decompression
```
bucket-src
└─── src/
     ├─── a.zip
     └─── b.zip
bucket-dst
└── src/
      ├óΓé¼┼ô────a/
      | ──── a.txt
      ├óΓé¼╦£────b/
           |──── b.txt
ðŸ™' ðŸ™'

## Modify rules

Select the US3 specified storage, and click the Data Processing button on the right side to display all the decompression rules for that storage, select the rule you want to modify
Select the rule you want to modify! [image](/images/zip/modify-rules.png)
After you click Modify, the Modify window will pop up, modify the fields you want to modify in the corresponding window
! [image](/images/zip/modify-rules-popup.png)

## Delete Rules

For unwanted zip package decompression rules, you can delete them manually by selecting US3 to specify the storage space, clicking the data processing button, and selecting the rule you want to delete, which will not automatically trigger decompression after deletion
! [image](/images/zip/delete-rules.png)
According to the pop-up window, select OK to delete or cancel the operation
! [image](/images/zip/deletion-rules-popup.png)

