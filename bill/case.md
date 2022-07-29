

# Billing cases

## Case 1
User A uses domestic US3 with UCDN acceleration service turned on. daily consumption on 1 Dec 18: File storage (standard type) capacity is 2.5TB. generating 200GB of CDN return traffic, This user's acceleration scenario generates 900GB of CDN download traffic.
Cost calculation for the day.
The total costs on the US3 product side are as follows.
Storage space cost: 2.5 \ 1024GB \ * 0.004 Yuan/GB/day = 10.24 Yuan
CDN back to the source traffic cost: 200GB \* 0.15 yuan/GB = 30 yuan
Total cost: $10.24 + 30 = $40.24
The total cost on the UCDN product side is as follows
CDN download traffic cost = 900GB \* $0.34/GB = $306
US3+UCDN total cost: $40.24 + 306 = $346.24

Note: Users who have enabled the UCDN acceleration service will also incur a CDN download traffic cost, which will be counted and billed on the UCDN product The billing price is detailed in: [UCDN Product Price](https://docs.ucloud.cn/ucdn/charge).

## Case 2
User B uses US3 without UCDN acceleration service turned on. Daily consumption on 1 Dec 18: file storage (standard type) capacity is 2.5TB, generating 150GB of outbound traffic during idle hours and 800GB of outbound traffic during busy hours.
Cost calculation for the day.
Storage space cost: 2.5\* 1024 GB\* 0.004 Yuan/GB/day = 10.24 Yuan
Outflow traffic cost: 150GB \* 0.25 Yuan/GB + 800GB \* 0.45 Yuan/GB = 397.5 Yuan
Total cost: 10.24 + 397.5 = 407.74 RMB

## Case 3
User C uses US3 without UCDN acceleration service. The daily consumption on 1 Dec 18: 1TB of file storage (standard type), 1TB of file storage (low The daily consumption on 1 Dec 18: 1TB of file storage (standard type), 1TB of file storage (low frequency type), and 0.5TB of file storage (archive type). 500GB of outbound traffic of standard type, 10GB of outbound traffic of low frequency type, 15GB of unfrozen data of archive type, and 10GB of outbound 500GB of outbound traffic of standard type, 10GB of outbound traffic of low frequency type, 15GB of unfrozen data of archive type, and 10GB of outbound traffic of archive type are generated during busy hours.
Same day cost calculation.
Storage space cost: 1 \* 1024GB \* $0.004/GB/day + 1 \* 1024GB \* $0.002/GB/day + 0.5 \* 1024GB \* $0.0008/GB/day = $10.24
Outbound traffic cost: (500GB + 10GB + 10GB)\* $0.45/GB = $234
Low-frequency type retrieval fee: 10GB \* $0.03/GB = $0.3
Archive type unfreeze fee: 15GB \* $0.06/GB = $0.9 (Note: if the unfreeze operation is performed, the data unfreeze fee will be charged even if the data is not retrieved)
Total cost: 10.24 + 234 + 0.3 + 0.9 = $245.44
