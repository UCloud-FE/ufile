

# Product Price

This article introduces the billing and pricing criteria for US3 object storage services. mainland China" and "Hong Kong, Taipei and Overseas".

## Mainland China

| Item | Billing | Standard Type Unit Price | Low Frequency Type Unit Price | Archive Type Unit Price |
| ------ | --------- | ----------- | ----------- | ---------- |
| Storage Charges | Data Storage | $0.004/GB/day | $0.002/GB/day | $0.0008/GB/day |
| Traffic Charges | Inbound/outbound traffic | Free | Free | Free | Free
| ::: | Inbound Outbound Traffic | Free | Free | Free | Free
| ::: | Outbound traffic | 00:00-08:00 (idle): $0.25/GB | 00:00-08:00 (idle): $0.25/GB | 00:00-08:00 (idle): $0.25/GB | 00:00-08:00 (idle): $0.25/GB | 00:00-08:00 (idle): $0.25/GB | 00:00-08:00 (idle): $0.25/GB
| ::: | ::: | 08:00-24:00 (busy): $0.45/GB | 08:00-24:00 (busy): $0.45/GB | 08:00-24:00 (busy): $0.45/GB | 08:00-24:00 (busy): $0.45/GB | 08:00-24:00 (busy): $0.45/GB | 08:00-24:00 (busy): $0.45/GB
| ::: | CDN Back to Source Flow Out Traffic | $0.15/GB | $0.15/GB | $0.15/GB |
| Request Cost | PUT Type Request | $0.01/10,000 | $0.1/10,000 | $0.1/10,000 |
| ::: | GET type request | $0.01/10,000 | $0.1/10,000 | $0.1/10,000 |
| Data Retrieval | Data Retrieval | / | $0.03/GB | Free when file is unfrozen |
| ::: | Data Unfreezing | None | None | $0.06/GB |
| Image Processing | Basic Image Processing | $0.025/GB | $0.025/GB | $0.025/GB |
| ::: | Image Advanced Compression | $0.1/1000 | $0.1/1000 |$0.1/1000|
| Minimum Storage Term | Minimum Storage Term | None | 30 days | 60 days |
| Minimum File Size | Minimum File Size | None | 64KB | 64KB |

<br> **Currently, US3 China's available zones include: North China I, North China II, Shanghai II, and Guangzhou.

**Note: [Image Processing] will be billed from July 1st, 2022.

## Hong Kong, Taipei and Overseas

| Item | Billing Item | Standard Type Unit Price | Low Frequency Type Unit Price | Archive Type Unit Price |
| ------ | --------- | ----------- | ----------- | ---------- |
| Storage Charges | Data Storage | $0.004/GB/day | $0.002/GB/day | $0.0008/GB/day |
| Traffic Charges | Inbound/outbound traffic | Free | Free | Free | Free
| ::: | Inbound Outbound Traffic | Free | Free | Free | Free
| ::: | Outbound traffic | $0.4/GB | $0.4/GB | $0.4/GB |
| $0.4/GB | $0.4/GB | $0.4/GB
| Request cost | PUT type requests | $0.01/million | $0.1/million | $0.1/million |
| Request Fee | GET Type Request | $0.01/10,000 | $0.1/10,000 | $0.1/10,000 |
| Data Retrieval | Data Retrieval | Free | $0.03/GB | Free when file is unfrozen |
| ::: | Data Unfreezing | None | None | $0.06/GB | 1 day)
|1 day) |||||
| Image Processing | Basic Image Processing | $0.025/GB | $0.025/GB | $0.025/GB |
| ::: | Advanced Image Compression | $0.1/1000 | $0.1/1000 |$0.1/1000|
| Minimum Storage Term | Minimum Storage Term | None | 30 days | 60 days |
| Minimum File Size | Minimum File Size | None | 64KB | 64KB |
| Overseas Dedicated Traffic | Outbound Traffic | $4/GB | $4/GB | $4/GB |

**Currently, US3 Hong Kong, Taipei and overseas availability zones include: Hong Kong, Los Angeles, Singapore, Jakarta, Taipei, Lagos, Sao Paulo, Dubai, Frankfurt, Ho Chi Minh City, Washington, Mumbai.

**Note: [Image Processing] will be billed from July 1st, 2022. **
## Additional Notes

Both low-frequency storage and archive storage types have a minimum storage period. Deleting, modifying, or overwriting files earlier than the minimum storage period requires making up the storage fees for the remaining days that have not yet reached the minimum storage period.

2. For both low-frequency storage and archive storage types, storage space is calculated according to 64KB file size for single file less than 64KB, and The actual size for files over 64KB.

For archival storage type, data unfreezing operation using RESTORE requires a data unfreezing fee. After the unfreeze is completed, no additional charge will be made for data retrieval, only the normal outflow traffic charge. After the thawing operation is completed, the data will be retained for 3 days, and no retrieval fee will be charged for retrieving the thawed data.

When using the cross-region replication function, the outflow traffic cost will be incurred, and the storage space of each region will be charged The globalized storage space function has been taken offline (it is recommended to The globalized storage space function has been taken offline (it is recommended to use the cross-region replication function), the original globalized storage space billing items and billing rules remain unchanged for the time being, and the old price and billing method are maintained.

For data whose storage type is low-region replication function, the original globalized storage space billing items and billing rules remain unchanged for the time being, and the old price and billing method are maintained.
   For data whose storage type is low-frequency type, the retrieval fee = download file size * retrieval fee unit price + outgoing traffic cost, and the retrieval fee will be charged regardless of intranet or extranet download.
   For data whose storage type is archive type, unfreeze fee = unfreeze file size * unfreeze fee unit price, and outflow traffic fee is charged for downloading the unfreezed data.
   The data retrieval fee is independent of the traffic cost, if the data retrieval process generates outbound traffic cost then it will be charged normally, No traffic cost will be charged for inbound download; during the thawing period, no additional thawing fee will be charged.
