# Data archiving solutions

## Background
In the field of data archiving, the traditional tape library or Blu-ray disc library media has been the preferred choice in the past. Once the data is stored on these tapes or discs, it means that the data goes into some obscure corner of the data center and will usually go into a "dormant" stage if not necessary, and some data will not be read or used for decades.
Nowadays, in the context of digital economy, cold data value mining has received more and more attention, flexible data retrieval, quasi-real-time data retrieval capability has also become the core demand of the new era of data archiving scenarios, while users also began to focus on the value of data tiering from the perspective of TCO cost optimization.

This article describes how to flexibly use different storage types of US3 to store data to achieve the purpose of reducing the long-term data storage costs.

## US3 Archive Types

The UCloud archive type achieves a storage unit price of 0.024 RMB/GB/month, which is 30% lower than the price of similar products in the market, while guaranteeing data reliability of more than 11 9 and data availability of more than 99.9%.
The archive storage type requires an unfreeze data operation before using the data, and the unfreeze data will incur an unfreeze fee of $0.06/GB, and the data unfreeze operation will be executed within 5 minutes. The current data retrieval method provided by the US3 archive type is fast retrieval, and US3 plans to provide inexpensive bulk unfreeze operations to reduce data unfreeze costs.
Once the thaw is completed using RESTORE, there is no additional charge for data retrieval, only the normal outbound traffic charge. After the thawing operation is completed, the data will be retained for 3 days, and no retrieval fee will be charged for retrieving the thawed data.
If you need to know more about the pricing of US3 archive type, we recommend you to refer to Case 3 in [product price](/ufile/bill/billing) and [billing case](/ufile/bill/case).

## Storing Cold Data with US3 Archive Type

If you need to store cold data using the US3 archive type, US3 supports either direct storage to the archive type by specifying the storage type at upload time, or manual storage type conversion using file storage type conversion.
US3 also supports you to configure lifecycle policies for data stored in US3 standard types or low-frequency types to achieve periodic automatic cooling.

### Upload files directly to archive storage

1. Uploading files via the US3 web console

Select the specified storage, enter the file management page, and click "Upload File" on the file management page to bring up the upload file window.

! [image](/images/file-management4.png)

After adding the files you want to upload, you can check "Archive storage" on the right side of the "Select storage type" option to perform the upload operation, and the files will be uploaded to the archive storage.

2. File upload via Upload API

In the upload class API provided by US3, the X-Ufile-Storage-Class field in the Request Headers is used to determine the storage type. If the field in the default request header is empty, the file will be uploaded to the standard type of storage by default and will be billed according to the standard type. If you need to upload files directly to archive storage, you need to specify the value of X-Ufile-Storage-Class as ARCHIVE.

If you need to know more about the US3 upload class API, you can refer to [Upload File](https://docs.ucloud.cn/api/ufile-api/put_file), [Form Upload](https://docs.ucloud.cn/api/ufile-api/post_file ), [Initialize Slice](https://docs.ucloud.cn/api/ufile-api/initiate_multipart_upload) API documentation.

### Manually change the storage type to archive storage

1. Modify the storage type via US3 web console

Select the specified storage space, enter the file management page, and click "Modify Storage Type" in the drop-down list of actions to the right of individual files.

! [image](/images/file-management9.png)

You can manually change the storage type to archive storage by selecting "Archive Storage" in the pop-up window.

Modify the storage type via the File Storage Type Conversion API

US3 provides [File Storage Type Conversion](https://docs.ucloud.cn/api/ufile-api/class_switch) API, you can use this API to convert the storage type of files.

### Set the lifecycle policy to convert files to archive storage

You can save storage costs by enabling the lifecycle feature of the storage space (Bucket) to automatically convert all files or specific prefix files in the storage space to archival storage on a regular basis.

Please refer to [Lifecycle](/ufile/guide/lifecycle) for details on how to set up the lifecycle feature.

## Scenarios for archival storage

### Multimedia archiving scenario

In these scenarios, a 1080P HD camera storage requires 45G of capacity a day, and a video website generates more than TB of data per day; a UCloud customer previously used Blu-ray storage, which is expected to reach 16.4PB by 2024, requiring about 8 Blu-ray disk cabinets. It takes up a whole row of cabinet space in the server room, which is a huge cost for the customer.

UCloud's new generation archive storage can provide write bandwidth no less than standard storage, realize asynchronous data retrieval at the minute level, and online lookback; and adopt redundancy policy of corrective code to ensure data security and safety. Combined with US3's lifecycle conversion between different storage types, users can also quickly convert data from hot to warm to cold storage types to complete automated data lifecycle management.

### Historical data compliance storage

US3 provides [database backup solutions](/ufile/solutions/backup) to help users streamline the backup process in the face of the increasing number of enterprise database backup scenarios. For users who need to clean backups regularly and reduce backup costs, US3 supports data lifecycle management, which enables automated periodic data cleanup and regular transfer to archival storage. For users who need a higher level of security, US3 supports cross-region replication to help users complete off-site backups of data.

In the log archiving scenario of e-commerce platform, US3 also provides ElasticSearch access and database backup function, and when the data volume increases, the historical data is archived to the archival storage in a unified way to reduce storage costs.

### Big data, AI analytics data archiving

According to research institutions, the bio-economy has reached $1.5 billion in 2020. Take the gene sequencing of oncology disease as an example, the DNA sample data of a single patient can reach 560GB, if calculated based on more than 18 million cancer cases per year, the use of gene analysis technology will generate 10PB of tumor gene sample data per year. US3 archival storage can provide long-term archival storage for a large amount of biological information, IoT real-time analysis data, and other scenarios to reserve data for future medical research and industrial intelligence.

In the context of new infrastructure, as new technologies and new scenarios continue to converge, industries such as online education, cloud gaming, autonomous driving, smart communities, and smart manufacturing will generate more and more data. US3 archival storage is geared towards future data hierarchical storage scenarios and uses a new self-research storage architecture to reduce users' hardware costs and operational costs, allowing users to store data at a lower price and in a more reliable manner assets and accumulate wealth for future mining of the value of data production factors.
