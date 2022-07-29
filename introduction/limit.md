

# Usage restrictions

## Supported geographies

| Geographic | Single Geographic Space Management | Cross-Region Replication |
| --- | ------- | ------- |
| North China I | Support | Support | Support
| North China II | Support | Not Support |
Support | Support | Support | Support | Support | Support | Support | Shanghai II | Support | Support | Support | Support | Support
Support | Support | Support | Support | Support | Guangzhou
| Support | Support | Not Supported
| Support | Support | Not Supported
| Support | Support | Not Supported
Support | Not Supported | Support | Jakarta | Support | Not Supported
| Support | Not Supported
| Support | Not Supported
| Support | Not Supported
| Support | Not Supported
| Support | Not Supported
| Support | No Support
| Washington, D.C. | Support | No Support
| Support | Not Supported
| Support | No Support
| Tokyo | Support | No Support
| Bangkok | Support | Not Support

## HTTPS Access Restrictions

Support HTTPS access for secure transmission.

Note: Currently, HTTPS is not supported for mirroring back to the source and custom domains.

## Upload file size limit

The file size of upload via console, upload file PutFile, form upload PostFile, and second upload file UploadHit cannot exceed 512MB.

## Image Processing Restrictions
|-- | Size limit | Concurrency limit |--
|-- |------- |------- |
|Beijing | 20M |50 |
|Beijing | 20M |50 |Shanghai II | 20M |10 |
|Beijing | 20M |50 |Shanghai II | 20M |10 |Beijing
| Hong Kong | 20M |10 |
|20M |10 |Hong Kong | 20M |10 | Taipei | 20M |10 |
| Other regions | 20M |5 |


Note: For larger requests, please contact your account manager or technical support.

## Other Restrictions
|Restrictions |Description |
|---------------- | ------------------------------------------------------------------------ |
|Archive storage type |Access requires unfreezing, and it takes 1 minute to restore the already stored data from frozen state to readable state. Selected
|Storage space (Bucket) |The total number of storage spaces created by the same account in the same locale cannot exceed 20. |:::: |Storage
|Once a storage space is created, its name, location, and storage type cannot be modified. |:::: |A single storage space cannot be created.
|::: |There is no limit to the capacity of a single storage space. |::: |The capacity of a single storage space is not limited.
<br>The default bandwidth for uploading or downloading files from the same account in the same region is: <br>Internal network upload and download speed <br>Internal network upload and download speed : 10Gbit/s in mainland China, 1Gbit/s in other regions;<br>External network upload and download speed: 1Gbit/s.<br>If your business (such as big data <br>If your business (such as big data offline processing, etc.) has a larger bandwidth requirement, please contact Technical Support.
|QPS (write) |1000 requests per second (non-cap) for normal users, 100 for overseas, please contact technical support if your business has a larger QPS
|QPS (Read) -2000 requests per second for normal users (non-capped), 100 for overseas, please contact technical support if your business has larger requirements.
|The maximum number of file deletions for the console is 1000, 100 for overseas. If you have larger deletion needs, please contact technical support.
|Domain Name Binding - Domain names bound in mainland China must be filed with the Ministry of Industry and Information Technology, while domain names bound in other regions do not need to be filed with the Ministry of Industry and Information Technology. Each storage space can be bound to a maximum of 100 |Domain name binding



