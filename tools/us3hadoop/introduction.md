# Tool Introduction

## Overview

US3Hadoop adapter is to solve the problem of Hadoop access to UCloud object storage US3, implement the standard Hadoop file system, support Hive, Spark, Flink and other big data computing frameworks can read and write data stored on US3 like access to the HDFS file system.

## Principle Description

The adapter is an adapter component that implements the storage access interface FileSystem provided by Hadoop, similar to the DistributedFileSystem implemented by HDFS and S3AFileSystem implemented based on the AWS S3 protocol. adapter directly sends IO, index requests to US3, the architecture is shown in the figure below.

! [](/images/hadoop_no_mds.png)

The architecture is suitable for **backup scenarios** and **small-scale computational analysis scenarios**.

## Versions and runtime environments

### Software Versions

Current version: 1.1.0

### Running environment

  - hadoop-2.6.0
  - hadoop-2.8.3
  - hadoop-2.8.5
  - hadoop-3.1.1