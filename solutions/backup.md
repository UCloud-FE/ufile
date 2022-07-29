# Database backup solutions

## Background
As a cloud storage application for massive unstructured data, object storage can effectively help users to reduce backup process, lower backup cost and effectively improve user experience in the face of rising database backup scenarios.
This article introduces how to complete database backup based on US3.

## Application Scenarios
At present, there are three main types of US3 database backup scenarios as follows.

1. backup and recovery: the backup method is recommended to use Filemgr to backup to US3.
Filemgr supports local backup recovery and streaming backup recovery, and the streaming function can help users to complete data backup and recovery without landing.

2. Tiered storage: For users who need to clean up backups regularly and reduce backup costs, US3 supports lifecycle function.
Specifying the lifecycle rules through the console can help users complete: 1. regular cleanup; 2. regular transfer to low frequency; 3. regular transfer to archive.

3. Offsite backup: For users who need higher security level, US3 supports cross-region replication function.
By configuring the cross-region replication function through the console, it can help users complete off-site backup of data while uploading backups.

! [image](/images/backup1.png)

## Solution advantages
1. Use Filemgr to perform streaming backup and streaming recovery to complete no-drop backup and recovery, which can avoid disk drop operation.

2. Use US3 [lifecycle](/ufile/guide/lifecycle) function with periodic deletion, low-frequency storage and archival storage to achieve hierarchical data storage and help users save storage costs.

3. Use US3 [cross-region replication](/ufile/guide/multisite) to perform off-site disaster recovery for backup data and improve backup data security.

## Solution Implementation
### Use Filemgr for streaming backup and streaming recovery
1. Download Filemgr, [migration tool](ufile/tools/tools/tools_file)

2. Configure config.cfg in Filemgr directory, and refer to [region and domain](/ufile/introduction/region) for proxy host domain name

```
{
    "public_key" : "paste your public key here",
    "private_key" : "paste your private key here",
    "proxy_host" : "www.cn-bj.ufileos.com", // proxy host please fill in the corresponding geographical domain name
    "api_host" : "api.spark.ucloud.cn"
}
````
3. Use Filemgr to restore backup, here is the most simple command, other backup commands please combine with your business analogy to achieve

```
bash
# Note that if you want to use low frequency storage (IA) or cold storage (ARCHIVE), please specify it in the command parameter storageclass, which supports three values: STANDARD, IA, ARCHIVE
# Note that if you specify the storageclass parameter as ARCHIVE when backing up, you need to restore the file in advance
. /filemgr-linux64 --action restore --bucket <bucketName> --key <backupKey>

# Logical backups
    # Full library backup
    mysqldump -A | . /filemgr-linux64 --action stream-upload --bucket <bucketName> --key <all-backupKey> --file stdin --threads <threads> --retrycount <retry> -- storageclass <storage-class>
    # split-library backup
    mysqldump -B database1 database2 | . /filemgr-linux64 --action stream-upload --bucket <bucketName> --key <part-backupKey> --file stdin --threads <threads> --retrycount <retry> -- storageclass <storage-class>

# Logical Backup Recovery
    # full-repository backup recovery
    . /filemgr-linux --action stream-download --bucket <bucketName> --key <all-backupKey> --threads <threads> --retrycount <retry> 2>. /error.log | mysql
    # split-backup recovery
    . /filemgr-linux --action stream-download --bucket <bucketName> --key <part-backupKey> --threads <threads> --retrycount <retry> 2>. /error.log | mysql

# --------------------------------------------------------------------------------------------------------------------------------- ------------------------------------------------------
# xtrabackup physical backups
    # Full backup
    innobackupex --stream=tar | . /filemgr-linux64 --action stream-upload --bucket <bucketName> --key <full-backupKey> --file stdin --threads <threads> --retrycount <retry> -- storageclass <storage-class>
    # incremental backups
    innobackupex --stream=tar --extra-lsndir=/data/backup/chkpoint /data/backup/tmp/ | . /filemgr-linux64 --action stream-upload --bucket <bucketName> --key <incre-backupKey-base> --file stdin --threads <threads> --retrycount < retry> --storageclass <storage-class> # full backup
    innobackupex --stream=xbstream --incremental --extra-lsndir=/data/backup/chkpoint --incremental-basedir=/data/backup/chkpoint /data/ backup/tmp/ | . /filemgr-linux64 --action stream-upload --bucket <bucketName> --key <incre-backupKey-incre> --file stdin --threads <threads> --retrycount < retry> --storageclass <storage-class> # incremental

# xtrabackup physical backup recovery, need to transfer the original DB data in advance, need to restart the service after backup recovery
    # Full backup recovery
        # full-backupKey is the key used for full backup.
        . /filemgr-linux --action stream-download --bucket <bucketName> --key <full-backupKey> --threads <threads> --retrycount <retry> 2>. /error.log | tar xf - -C /data/backup/full/
        innobackupex --apply-log /data/backup/full/
        innobackupex --copy-back --rsync /data/backup/full/

    # incremental backup recovery
        # full-backupKey is the key used for full backups, incremental-backupKey is the key used for incremental backups.
        . /filemgr-linux --action stream-download --bucket <bucketName> --key <incre-backupKey-base> --threads <threads> --retrycount <retry> 2>. /error.log | tar xf - -C /data/backup/base/
        innobackupex --apply-log --redo-only /data/backup/base/
        . /filemgr-linux --action stream-download --bucket <bucketName> --key <incre-backupKey-incre> --threads <threads> --retrycount <retry> 2>. /error.log | xbstream -x -C /data/backup/incre
        innobackupex --apply-log /data/backup/base --incremental-dir=/data/backup/incre
        innobackupex --copy-back --rsync /data/backup/base

# lvm snapshot physical backup recovery
    # backup, /snap-lvm0 is the backup lv mount point, /data-lvm0 is the source lv mount point
    tar czf - /snap-lvm0/* | . /filemgr-linux64 --action stream-upload --bucket <bucketName> --key <lvmsnap-backupKey> --file stdin
    # restore, local data is cleared, and --strip-components is used to remove the first level of directories created when compressing snapshots
    . /filemgr-linux64 --action stream-download --bucket <bucketName> --key <lvmsnap-backupKey> 2>. /error.log | tar xzf - -C /data-lvm0/ --strip-components 1
    # merge historical snapshots, /snap-lvm0 is the snapshot lv mount point
    . /filemgr-linux64 --action stream-download --bucket <bucketName> --key <lvmsnap-backupKey> 2>. /error.log | tar xzf - -C /snap-lvm0/ --strip-components 1
    lvconvert --merge <vg>/<snap-lv>
For users with special needs, the corresponding logical backup commands are provided here, for other backup commands please implement by analogy.

# compress
    # backup
    mysqldump -A | gzip | . /filemgr-linux64 --action stream-upload --bucket <bucketName> --key <all-backupKey> --file stdin --threads <threads> --retrycount <retry> -- storageclass <storage-class>
    # recover
    . /filemgr-linux --action stream-download --bucket <bucketName> --key <all-backupKey> --threads <threads> --retrycount <retry> 2>. /error.log | gzip -d | mysql

# encrypt
    # backup, use aes256, specify the password file key file in the backup path for compression
    mysqldump -A | openssl enc -e -aes256 -in - -out - -kfile <key file> | . /filemgr-linux64 --action stream-upload --bucket <bucketName> --key <all-backupKey> --file stdin --threads <threads> --retrycount <retry> -- storageclass <storage-class>
    # recover
    . /filemgr-linux --action stream-download --bucket <bucketName> --key <all-backupKey> --threads <threads> --retrycount <retry> 2>. /error.log | openssl enc -d -aes256 -in - -out - -kfile <key file> | mysql
```

**Remarks:**
If you do not want to terminate the task by exception, please set the retrycount parameter to a larger value, the default is 10. Each failed execution will start retry, and each retry will wait for 5s from the 5th retry, please calculate the number of retry reasonably.

### Implement periodic deletion using life cycle
1. Open the object storage console and enter the bucket details page used for backup

! [image](/images/backup2.png)

2. Click the lifecycle tab to enter the lifecycle configuration page

! [image](/images/backup3.png)

3. Configure periodic deletion tasks for the bucket

4. when the backup file exceeds the configured period, the file will be deleted automatically

### Using cross-region replication for offsite disaster recovery
1. Open the Object Storage Console and go to the details page of the bucket used for backup

! [image](/images/backup4.png)

2. Click the open region replication tab to enter the cross-region replication configuration page

! [image](/images/backup5.png)

3. Configure the cross-region replication task for the bucket

4. When backup files are created under this bucket, the files will be automatically synchronized to the configured offsite bucket
