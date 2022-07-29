

# Migration Tool (historical version)

## Introduction
ufile-import is a tool provided by Object Storage US3 for migrating data to US3 storage (Bucket). You can deploy ufile-import on a local service or cloud host to easily migrate data from your other cloud storage to US3.

The new US3SYNC migration tool has been released, please refer to: [US3SYNC Migration Tool](ufile/tools/us3sync/us3sync)

### Applicable cases

* Migration of AliCloud object storage data to US3 object storage
* Migration of Qiniu cloud object storage data to US3 object storage
* Data migration before different buckets of US3 Object Storage
* Migration of S3 protocol-enabled object storage to US3 object storage

### Preparation
1. According to the total size of the files to be migrated, choose the cloud host with the right hard disk. You must ensure that the hard disk storage capacity is larger than the file migration data, otherwise the hard disk size may not be sufficient, resulting in incomplete migration data.

Example: Suppose you have a total of 100G files to be migrated and the number of concurrent files to be processed is 40 (i.e., the "concurrent" parameter in ufile-import.json, read on to understand the use of this parameter.) If the maximum size of a single file is 1G, you need at least 40 /* 1 /* 2 = 80G to cache the temporary files during the download process, to ensure that there is enough hard disk capacity to cache the temporary files during the migration process, otherwise the migration file may be incomplete.

## Installation steps

1. Download the installation package
For Linux 64-bit operating system, please download
Download address: https://github.com/ufilesdk-dev/ufile-import/archive/master.zip

2. Install the program.

Go to the directory where you downloaded the installation package and unzip the file.

    unzip master.zip
    cd ufile-import-master
    tar zxvf ufile-import.tgz

3. Start the redis service.

The service relies on the redis service, and the installation package already contains the configuration for the redis service, so you can start it directly.

     1 cd ufile-import
     2 cd redis
     3 . /start.sh

You can check the status of the redis service by executing the `. /ps.sh` command to check the status of the redis service. The redis service starts normally with the following status:

     root 5318 1 0 14:50 pts/0 00:00:15 . /redis-server 127.0.0.1:6379

Refer to [US3 Migration Tool User Guide](https://github.com/ufilesdk-dev/ufile-import) for more details on using the tool.
