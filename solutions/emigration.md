# Data Migration Solutions

## Background
Object storage has the advantages of large capacity, low cost, high scalability and high reliability, and faces the problems of storage capacity bottleneck and cost increase due to incremental data. Users can migrate data from third-party storage clusters to US3's storage space by means of data migration, taking full advantage of object storage's on-demand charging without purchasing additional hardware resources to archive data for optimal cost.
This article describes how to migrate data from other source sites to US3.

## US3 Mirror Back to Source feature

After the user creates a storage space in US3, by configuring the back source configuration of the storage space, US3 will first look for the file locally in US3 when it receives an access request, and if the file does not exist, it will back source to the source station specified by the user to get the file and store a copy locally in US3. The next time the same file is accessed it will be returned directly to the user from US3 locally.

For details on how to set this up, please refer to.
[Mirror back to source](https://docs.ucloud.cn/ufile/guide/mirror)

Advantages.

1. Customer requests can be switched to US3 quickly and seamlessly.

2. Data migration to US3 is imperceptible.

Disadvantages.

1. Some of the files that are not accessed cannot be migrated to US3.

2. Accessing files that do not exist in US3 requires going back to the source, and the access latency will be slightly increased.

## US3 Data Migration Tool

### Introduction
US3SYNC is a tool provided by Object Storage US3 to migrate data to US3 storage (Bucket). You can deploy US3SYNC on a local service or cloud host to easily migrate data from your other cloud storage to US3.

### Application
AliCloud Object Storage data migration to US3 Object Storage
Migration of Qiniu Cloud Object Storage data to US3 Object Storage
Migration of data before different buckets of US3 Object Storage
Support s3 protocol object storage migration to US3 object storage

For more details, please refer to: [US3SYNC Migration Tool](https://docs.ucloud.cn/ufile/tools/us3sync/introduction)