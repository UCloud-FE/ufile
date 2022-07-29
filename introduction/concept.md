# Product Overview
Object Storage (US3) is a service that provides unstructured data storage for Internet applications.  Compared to traditional hard disk storage, object storage has the advantages of uncapped storage, support for high concurrent access, and lower cost.
Its data persistence is no less than 99.999999999%, and the availability of standard storage services is no less than 99.95%.
You can easily move large amounts of data in and out of US3 using the APIs, SDK interfaces or US3 migration tools provided by US3. You can also choose the lower -cost, longer-lasting low-frequency storage type and archive type US3 services for backup and archiving of infrequently accessed data.
<video id="video" length=1000 width=800 controls="" preload="none" poster=" https://static.ucloud.cn/c8f75929f40abde64c0a5d3b58cf440b.png  ">
<source id="mp4" src=" http://caozuozhinan.cn-bj.ufileos.com/ Video 1 US3 introduction.mp4 ">
</video>
## Key Concepts
### Object storage space (Bucket)
An object storage space (or storage space for short) is an organizational management unit for files, and a file necessarily belongs to one of the spaces.  Space names are globally unique and cannot be modified.
Each account can create up to 20 buckets, and there is no limit to the number of files in a bucket.
Users can set the storage space as public or private to control the access rights of files in the storage space.
### Private space
All files can only be accessed with the owner's API key authorization for all operations.
### Public space
All file downloads can be accessed directly via URL.  Uploads, deletions, and lists still require API key authorization for access.
### Objects/Files (Object)
A file is a logical storage unit for a storage space.  For each account, each file stored in that account is identified by a unique pair of storage space ( Bucket) and key (Key).
### File name (Key)
File name is the name of the corresponding file, globally unique in the storage space.  Uploading a file with the same file name will cause the original file name file to be overwritten.
When downloading files, users only need to know the domain name of the download outlet, not to know exactly which device in which server room the file will be Simply enter the corresponding URL in your browser to access it.
### File name naming convention
Use UTF-8 encoding
Length must be between 1-1023 bytes
It can start with "/" character, but "{}\^\[\]&lt;&gt;\#\~%" is not allowed.
### Access domain name (Endpoint)
Endpoint represents the access domain name of US3 external services.
When accessing different geographic areas, different domain names are required.  For details, see [Regions and Domains](/ufile/introduction/region).
### Token Key (API Access)
Token keys, which are pairs of public and private keys.
Users can create Token keys to grant different Token permissions for a bucket, and different Token can be distributed to different users to achieve subdivision permission management for the bucket.
In addition, Token keys can be set to be valid and can be deleted at any time to ensure the security of access to the Bucket.
### API key (API Access)
The API key is used for authentication when calling the API to prevent others from maliciously tampering with your request data, if the key is leaked, If the key is leaked, please reset it immediately, and you need to log out of the website after successful reset.
The API key contains both public and private keys.  Before the API request, you need to use the public key and private key to generate the signature.
To protect your account, please keep your private key safe and avoid spreading it.  Try not to use API key directly to access US3, it is more risky if it is Try not to use API key directly to access US3, it is more risky if it is leaked, we recommend using Token key.
### Region
Region indicates the physical location of US3's data center.  You can choose the region for data storage based on a combination of cost, request source, etc. . For details, please refer to US3's opened Regions.
### Single-region space management
The single-region object storage service solves the file storage problem for business architectures by creating multiple copies of user-uploaded data and enabling cross-room storage.
### Multi-geographic cross-regional replication
Users can set up cross-region replication for 2 or more buckets to achieve multi-territory data upload synchronization and multi-location data backup and disaster recovery;  it can also achieve the function of nearby upload by cooperating with the cyclic cross-region replication of custom domain name and cname to multiple buckets.
### Storage type
US3 provides three storage types: standard, low-frequency, and archive, comprehensively covering various data storage scenarios from hot to cold.  standard storage type provides highly reliable, highly available, high-performance object storage services and can support frequent data access;  the The standard storage type provides highly reliable, highly available, high-performance object storage services and can support frequent data access;  the low-frequency storage type is suitable for long-term storage of infrequently accessed data, and the storage unit price is lower than the standard type;  The archive storage type is suitable for archived data that needs long-term storage (more than six months is recommended), and the unit price is the lowest among the three storage types.  For details, please refer to [Introduction to Storage Types](/ufile/introduction/storage_type).
### Life cycle
Users can configure lifecycle deletion, which can delete files with specified prefixes periodically to save users' storage space.  automatic cooling function of lifecycle can cool down files with specified prefixes and automatically convert them to low-frequency storage, or convert them to archival storage to save users' storage costs.
## Related Services
Once you store your data in US3, you can use other products and services provided by UCloud to operate on it.  Here are some of the UCloud products and services you'll use regularly.
|Here are some of the UCloud products and services you'll regularly use.
---- |---- |---- |
|Cloud Hosting (UHost) |Provides simple and efficient cloud-based computing services with elastic and scalable processing power.  See [UHost product details page]( https://console.ucloud.cn/uhost/uhost ). | UHost
|Content Delivery Network (UCDN) |Caches source resources to edge nodes in each region for fast access to content close to you.  See [UCDN product details page]( https://console.ucloud.cn/ucdn/ucdndashboard ). | Data Lake Analytics (USQL)
|Data Lake Analytics (USQL) |Build a Serverless-based big data analytics solution for you to analyze and process your own data easily.  product details page]( https://console.ucloud.cn/usql/editsql ). | USQL
|Media Processing (UMedia) |Transcodes audio and video stored in UFile into formats suitable for playback on PC, TV, and mobile terminals.  the deep learning of massive data, it analyzes the content, text, voice, and scene of audio and video multimodally to achieve intelligent review, content Please see [Media Processing Product Details Page]( https://console.ucloud.cn/umedia/umediataskmanage  ). | The Media Processing Product Details Page]( ).
## Use US3
US3 provides a web service page for you to manage US3. You can log in to the US3 Management Console to operate the storage space and objects.  user's guide for information on operating the management console.
US3 also provides a rich API interface and SDK packages in various languages for flexible management of US3.
See [API list](/ufile/api_reference) and [SDK list](/ufile/tools/sdk) for flexible management of US3.
## US3 Pricing
Traditional storage service providers require you to purchase a pre-determined amount of storage and network transport capacity, and if you exceed that As your business grows, you will enjoy more infrastructure.  As your business grows, you will enjoy more infrastructure cost benefits.
For US3 pricing, see [metering and billing](/ufile/bill/new).