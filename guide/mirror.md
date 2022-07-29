# Mirror back to source

US3 provides mirroring back to source function. After configuring back to source rules, when the data you request from US3 storage space does not exist, You can set back to source for data retrieval requests in various ways to meet your needs for data hot migration and specific request redirection through back to source settings.

After setting the back source rules, you can match the URL of each GET request of US3 by the set rules, and then back source according to the set rules. Up to 5 rules can be configured, matching sequentially until a valid rule is matched.

* If mirroring write-back is configured, when GET operation is performed on a non-existent file, this file will be requested from the If mirroring write-back is configured, when GET operation is performed on a non-existent file, this file will be requested from the source address, returned to the user, and written to US3 at the same time.

## Usage Scenario

Most of your source files are stored in US3 storage space, and a small number of files are in the client source site, but there will still be a small number of files continue to write to the source site, you can directly go to the source site to source back to get the files through the mirror back function, so as to achieve smooth migration of business.

## Set the rules for back source

The user specifies the file prefix and the address of the source station, and when US3 has no files, US3 automatically grabs the files from the specified US3 automatically grabs the files from the specified source station according to the file prefix and saves them, and returns them to the user.

Select the corresponding space, and select the Mirror Back button in the right operation.

Select the corresponding space, and select the Mirror Back button in the right operation. [](/images/guide/setting-source-return-rules.png)

Click the Add back source rule button.

! [](/images/guide/click on back source rule v4.png)

Add back source rule screen.

! [](/images/guide/add-back-source-rules-interfacev4.png)

Fill in the file prefix and back source address, the file prefix is the real path of the specified source site, if it is the root directory, the prefix is empty .

Example: Access the files in the "test" folder of other source site through US3, set the back source rule with the prefix "test" and the back source address as the source site access address.

! [](/images/guide/ set prefix to test.png)

Access files in this folder on other source sites via US3.

! [](/images/access-source-station-files.jpg)

US3 also saves that file automatically.

! [](/images/guide/mirror-download-file-list v4.png)


## Remarks

1. 5 back source rules can be created under each single geographical storage space (Bucket), with unique and non-repeatable prefixes.

Globalized space management does not support setting back source rules at the moment.

3. After adding back to the source rules, the source domain name does not support HTTPS access.

4. Sub-accounts need to [open API key permission]([FAQ Object Storage US3_Document Center_UCloud Neutral Cloud Service Provider](https://docs. ucloud.cn/ufile/faq?id=After subaccount authorization enter file management page prompt: illegal authorization)), if the sub-account does not open the API key, US3 cannot upload back source files to the user's Bucket.
