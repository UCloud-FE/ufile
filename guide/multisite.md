

# Cross-regional replication

US3 provides cross-region replication function, you can select two different geographical storage (Bucket) to bind and synchronize files.

Cross-region replication is the automatic, asynchronous replication of files across storage in different US3 data centers (geographies), which copies file creation, update, and deletion operations from the source storage to the target storage in a different region.

The cross-region replication feature provides storage space cross-region disaster recovery capability while meeting your data replication needs. The files in the target storage are complete copies of the files in the source storage, and they have the same file name and content.

## Usage Scenarios

* The cross-region replication feature allows you to replicate the data across a certain distance. The cross-region replication feature allows you to replicate data between distant US3 data centers to meet these compliance requirements.

* Minimize latency: You have business needs in two geographic locations at the same time, and to reduce latency when accessing files, you can maintain You have business needs in two geographic locations at the same time, and to reduce latency when accessing files, you can maintain copies of files in US3 data centers that are geographically closer to your users.

* Data backup and disaster recovery: You have extremely high requirements for data security and availability, in case the US3 data center is destroyed due to a mega-disaster. You have extremely high requirements for data security and availability, in case the US3 data center is destroyed due to a mega-disaster, such as an earthquake, tsunami, or other force majeure, and you can still enable backup data from another US3 data center.

* Data replication: For business reasons, you need to migrate your data from one US3 data center to another.

* You have computing resources in two different data centers at the same time, and you may be able to maintain files in US3 on those You have computing resources in two different data centers at the same time, and you may be able to maintain files in US3 on those regional data centers at the same time through cross-regional replication.

## Caution

1. For two storage spaces that are in synchronization, since you can operate both storage spaces at the same time, the files copied from the storage space may overwrite the files with the same name in the target storage space, and the operation of overwriting files is irreversible.

2. Since cross-region replication uses asynchronous replication, it takes some time to replicate data to the target Bucket, usually ranging from a few Since cross-region replication uses asynchronous replication, it takes some time to replicate data to the target Bucket, usually ranging from a few minutes to a few hours, depending on the size of the data.

3. Mirror back to source and cross-region replication cannot be enabled at the same time.

If a storage bucket is configured with lifecycle rules, the lifecycle configuration only takes effect for the current storage, and files will not be If a storage bucket is configured with lifecycle rules, the lifecycle configuration only takes effect for the current storage, and files will not be deleted synchronously between storage spaces with cross-region replication enabled.

## Set cross-region replication rules

Select the corresponding space, enter the management interface, and select the cross-region replication function.

! [](/images/cross-region replication1.png)

Click the Add rule button to bring up the Add cross-region replication rule interface.

! [](/images/cross-region copy2.png)

Target Locale: Users can choose the locale of the storage space to be replicated. The two storage spaces for data synchronization must belong to two The two storage spaces for data synchronization must belong to two geographies, and data synchronization cannot be performed between storage spaces in the same geography.

The two storage spaces for data synchronization must belong to two geographies, and data synchronization cannot be performed between storage spaces in the same geography. 2. Target storage: Select the target storage to enable data synchronization. For example, if you have set the data of storage A to be synchronized to storage B, neither A nor B can establish data synchronization relationship with any other storage space at the same time. can establish data synchronization relationship with any other storage.

You can enable, disable, and delete the cross-region replication rules by using the other buttons under the action bar.

## Remarks

The cross-region replication function supports real-time data synchronization, and the addition, deletion and modification of data can be The cross-region replication function supports real-time data synchronization, and the addition, deletion and modification of data can be monitored and synchronized to the storage space of the target region in real time. For 2MB files, it can synchronize the information at minute level to ensure the final consistency of data on both sides and not to guarantee the consistency of data in the intermediate state.

Currently, only Beijing, Guangzhou and Shanghai support cross-regional replication function, and other regions will be supported in the future.
