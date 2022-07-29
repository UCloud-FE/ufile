# Storage type
US3 provides three storage types: standard, low-frequency, and archive, which are used for frequently accessed hot data, low-frequency accessed backup data, and archived data for long-term storage, comprehensively covering various data storage scenarios from hot to cold.
<video id="video" length=1000 width=800 controls="" preload="none" poster=" https://static.ucloud.cn/a5cdc8cbad57baa768e154b7fbbb04f2.png  ">
<source id="mp4" src=" http://caozuozhinan.cn-bj.ufileos.com/ Wangmengna 2 storage type.mp4 ">
</video>
## Standard storage
Standard storage provides highly reliable, highly available, high-performance object storage services with high throughput and low latency service Standard storage provides highly reliable, highly available, high-performance object storage services with high throughput and low latency service response capabilities to support frequent hot data access.
**Key Features:**
Data persistence: 99.999999999%
Availability: 99.95%
Access: Real-time access
Applicable scenarios: various social, sharing images, audio and video applications, large websites, big data analytics, mobile applications, game programs programs
## Low frequency storage
Low-frequency storage type provides high reliability, lower storage costs and lower access latency object storage services, suitable for long-term preservation of infrequently accessed data.  Storage unit price is lower than the standard type, low frequency storage has a minimum storage time and Storage unit price is lower than the standard type, low frequency storage has a minimum storage time and minimum object size, storage time shorter than 30 days early deleted will incur a fee.  storage space, data acquisition will incur costs.
**The data acquisition will incur costs.
Data persistence: 99.99999999999%
Availability: 99.9%
Access: Real-time access
Applicable scenarios: Long-term backup of various mobile applications, smart devices, government and enterprise business data, and enterprise data, Supporting real-time data access
## Archival storage
The archive storage type provides offline cold data storage with high reliability, very low storage cost and long-term preservation, suitable for archived data that needs to be kept for a long time (more than six months is recommended) and rarely accessed during the storage cycle.  Among the three storage types, archival storage has a minimum storage time and and minimum object size, the storage time is shorter than 60 days of the file File sizes below 64KB will be calculated in accordance with 64KB of storage space, and data acquisition will incur costs.
**Key Features:**
Data persistence: 99.99999999999%
Availability: 99.9%
Access: requires thawing before access, tens of seconds waiting time to recover from frozen state to readable state (first byte retrieved within 1 minute )
Applicable scenarios: Long-term preservation of archival data and other compliance documents archiving, medical images, scientific information, film and television material
## Storage type conversion
Automatic conversion of the following storage types is supported.
Standard storage conversion to low frequency storage
Standard storage to archival storage
3. low-frequency storage to archival storage
Notes.
1. For files that need to be reconverted from archive type to standard type or low-frequency type, the conversion of low-frequency type files to standard <br/>For example, If a user needs to reconvert a file that has been stored as a low-frequency type to a standard type, he can re-upload the file, specifying the standard For example,  if a user needs to reconvert a file that has been stored as a low-frequency type to a standard type, he can re-upload the file, specifying the standard storage type when uploading it, and the newly written file is the standard storage type.
2. For a file that has been dumped into an archive type, a RESTORE operation needs to be performed first to unfreeze it into a readable state before it can be Read.
## APIs supported by the storage type
|API |Standard Storage Type |Low Frequency Access Storage Type |Archive Storage Type
|----------------------- | ------ | -------- | -------- |
| Space Management | ::: | ::: | ::: |
| CreateBucket | Support | Support | Support |
| DescribeBucket | Support | Support | Support |
| UpdateBucket | Support | Support | Support
| DeleteBucket | support | support | support | support |
| File Management | ::: | ::: | ::: |
| support | support | support | support | support | support
| support | support | support | support | support
UploadHit | support | support | support | support | support
GetFile | support | support | support | support, need to unfreeze first |
| support | support | support | support, need to unfreeze first |
| support | support | support | support | support, need to unfreeze first
| Multipart Operations | :::: | ::: | ::: |
| InitiateMultipartUpload | Support | Support | Support | Support |
UploadPart | Support | Support | Support | Support |
| Support | Support | Support | AbortMultipartUpload
| Support | Support | AbortMultipartUpload | Support | Support | AbortMultipartUpload
| PrefixFileList | support | support | support | support | support |
| RestoreObject | Not Supported | Not Supported | Support |
| Other Features of the API | ::: | ::: | ::: |
| Log Management | Support | Support | Support | Support |
Support | Support | Support | Support | Support | Support | Image Processing | Support | Support | Support | Support | Support | Support | Support