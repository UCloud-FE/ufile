# Tool Introduction

## Overview

US3Hadoop Backup Tool (US3Distcp for short) is a tool built on MapReduce that automatically detects and can automatically synchronize data in HDFS with non-existent or inconsistent files in US3, so US3Distcp can directly replace Hadoop's own distcp to do data backup to US3 (currently it is recommended to synchronize with US3 via Hadoop distcp synchronization with US3Distcp checksum). Although the working principle is similar to Hadoop's own distcp, Hadoop distcp's file integrity checksum is based on HDFS checksum, and Hadoop distcp cannot directly use checksum to compare the data in HDFS with the data in US3 to see if it is the same. Therefore, to verify the integrity of files migrated from HDFS to US3, you need to use US3Distcp.

## Principle Description

US3Distcp synchronizes the relative position relationship of the source destination always with distcp. The tool will divide the whole execution phase into two phases.

- Detection phase: The detection phase will first construct the list of files to be synchronized, build the source and destination files to be detected in advance, and then decide the task split according to the detection strategy, which currently provides the following strategies.
  - whether the destination exists or not: this strategy is carried out by default and cannot be skipped, if the destination does not exist then the record is written to the reduce output file with `dst not found` indicating that the synchronization is needed because the destination does not exist.
  - whether the lengths are equal: like the `does exist` policy, it is done by default and cannot be skipped, this policy is logged if the source and destination sizes are not equal, with `size not match` indicating the reason why synchronization is needed.
  - whether the destination modification time is up-to-date: this policy is optional, if enabled it will compare the modification time of source and destination, if the source modification time is later than the destination modification time, the source and destination will be recorded with `modification time not match` indicating the reason why synchronization is needed.
  - checksum checksum: this checksum is not the concept of checksum in hdfs, but only a generic term for the file fingerprint information, currently supports `crc32c` and `md5` two algorithms, while taking into account that such checksum needs to consume bandwidth and calculate both ends, so consider the cost factor, provides a choice of two types of checksum.
    - Full checksum: this approach will compute the whole file on both ends using the chosen algorithm and then compare the final computed results.
    - Sample checksum: this way will check the head and tail 1MiB data separately, and then randomly select four 1MiBs for the middle part of the file to calculate the checksum (files smaller than 6MiB automatically use the full checksum). This type of checksum mainly takes into account the fact that the head and tail of some formatted data files in big data scenarios generally contain important self-describing information of the whole file, which ensures that the whole file will not be unusable due to data corruption at the head and tail, while there are usually synchronization points in the middle of the file, so even if there are several corruption points, the whole file will not be unusable.
- Synchronization phase: The synchronization phase will start mapreduce tasks for automatic synchronization according to the list of pending synchronization given in the detection phase, and there will be three job-level retries before all synchronization is successful. Currently the task splitting is also automated, each task is responsible for handling the synchronization of files up to 4.5GiB, but if a single file exceeds 4.5GiB, a map task is used to handle the file. The task splitting logic in this phase is the same as in the detection phase when performing checksum full checksum. This phase is also optional.

## Runtime environment

- Windows
- Linux
- MacOs


