# Tool Introduction

## Overview

US3FS is a tool to mount US3's storage (Bucket) to a local mount point in Linux/Windows system environment. After successful mounting, you can operate the files in the storage (Bucket) as if they were local files.

## Version and operating environment

### Software version

Current version: v1.6.7

### Running environment

- Linux.
  - CentOS 7.0 and above (can be viewed via `cat /etc/redhat-release`)
  - Ubuntu 16.04 and above (can be viewed via `cat /etc/issue`)
- Windows
  - Start WinFsp service

## Main Features

* Supports most of the POSIX file system features such as read; sequential write; permissions; UID/GID.
* Upload large files using US3's slice upload function.
* Support Etag and MD5 checksum to ensure data consistency.

## Usage limitations

* Reading files of archive type is not supported
* Random write/append write is not supported
* rename non-atomic operations
* Hard/soft linking is not supported
* When multiple clients mount the same US3 Bucket, users need to maintain data consistency by themselves.

