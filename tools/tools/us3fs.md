# US3FS File Mount Tool

- [Overview](# Overview)
- [Runtime Environment](#Runtime Environment)
- Download link](#Download link)
- Main features](#Main features)
- Usage restrictions](#Use restrictions)
- [Configure account access information](#Configure account access information)
- Usage](#usage methods)
  - Configure access rights](# Configure access rights)
  - [Set mount read-only](#Set mount read-only)
  - Enable logging](#Enable logging)
  - [Version update](#version update)
  - [Help](#Help)
- [Common Options](#Common Options)
  - [Performance Related](#Performance Related)
- [Options list](#Options list)
- Usage examples](#usage examples)

## Overview

US3FS is a tool that provides US3 storage (bucket) mounts to local mount points in a Linux environment. US3FS provides a shared file system access solution that not only enables random reads and writes to the object storage, but also gives you the ability to use the object storage on demand with unlimited capacity.

## Runtime Environment

us3fs is based on a user-state fuse implementation, and your machine needs to support fuse.

It is recommended that you run us3fs in the following environment.

* linux
  * ceontos 7.0 and above (can be viewed via `cat /etc/redhat-release`)
  * ubuntu 16.04 and above (can be viewed via `cat /etc/issue`)

* Network environment: us3fs is supported on UCloud intranet as well as Internet environment. In intranet environment, you can use intranet domain name to improve performance and stability.

## Download link

```
curl -o us3fs http://ufile-release.cn-bj.ufileos.com/us3fs%2Fus3fs
```

## Main functions

* Supports most of the POSIX file system features such as read, sequential write; permissions; uid/gid.
* Upload large files using US3's slice upload feature.
* Support etag and MD5 checksum to ensure data consistency.

## Usage limitations

* Reading files of archive type is not supported
* Random write/append write is not supported
* Only rename between files is supported
* No support for hard/soft links
* When multiple clients mount the same US3 storage, users need to maintain data consistency by themselves

-----

## Configure account access information

Edit /etc/us3fs/us3fs.conf and add the following information (if you don't have this directory, you need to create it yourself):

```yaml
bucket: your_bucket
access_key: ***********************************
secret_key: ***********************************
endpoint: ufile.cn-north-02.ucloud.cn (your_endpoint)
```
*Single space after colon *

* **bucket**: bucket name, needs to be the same as the mounted bucket name
* **access_key**: public key, supports both token secret key and api secret key modes
* **secret_key**: private key, supports both token secret key and api secret key modes
* **endpoint**: access domain name, see [locale and domain name](https://docs.ucloud.cn/ufile/introduction/region) for details. You need to remove `www.` when filling in

When you need to mount multiple stores on one machine, you can specify the account information by `-passwd=passwd_file`.

After downloading us3fs. Use `chmod +x us3fs` to add executable permissions, and move us3fs to the /bin directory if you need to execute it directly. Example.

```bash
chmod +x us3fs
. /us3fs --passwd=passwd_file <bucket> <mountpoint>

mv us3fs /bin/us3fs
us3fs --passwd=passwd_file <bucket> <mountpoint>
```

## Usage

* mount

```
us3fs [global options] <bucket> <mountpoint>
```

* unmount

```
umount <mountpoint>
```

-----

### Configure access rights

The default access permission for us3fs mount is the current mount user, if you need to allow other users/user groups to access the mount point, you can use the following parameters.

* `-o allow_other`: allow any user to access the file.
* `-uid=xxx`: specify the default user
* `--gid=xxx`: Specify the default user group
The uid/gid information of the user can be obtained by the `-id` command, the example is as follows.

```bash
// mount us3fs with default user and usergroup www under ubuntu account
ubuntu:~$ id www
uid=1001(www) gid=1001(www) groups=1001(www)

ubuntu:~$ us3fs --uid=1001 --gid=1001 -o allow_other <bucket> <mountpoint>
```

* If the following problem occurs with the mount

```bash
stderr:
/bin/fusermount: option allow_other only allowed if 'user_allow_other' is set in /etc/fuse.conf
```

Add ``user_allow_other`` to ``/etc/fuse.conf``

### Set mount read-only

Specify `-o ro` when mounting.

### Turn on logging

* --debug_u Enable us3fs logging
* --debug_fuse Enable user state fuse logging
  * centos
    Logs in /var/log/messages
  * ubuntu
    Logs in /var/log/syslog
* Specify -f when mounting, us3fs will mount in foreground mode and the logs will be output to the screen.

### Version update

Execute the following command.

```
us3fs --update
```

-----

## Help

View the parameters supported by us3fs via `us3fs -h`

``bash
❯ us3fs -h
us3fs - a single posix file system based on ufile
USAGE
  us3fs [global options] bucket mountpoint
FUSE
  --entry_timeout value How long to cache dentry for inode for fuse. (default: 0s)
  --attr_timeout value How long to cache inode attr for fuse (default: 0s)
  --dcache_timeout value How long to cache dentry for us3fs (default: 0s)
  --async_read Perform all reads (even read-ahead) asynchronously
  --sync_read Perform all reads (even read-ahead) synchronously
  -o value specify fuse option

OS
  --retry value number of times to retry a failed I/O (default: 5)
  --parallel value number of parallel I/O thread (default: 20)
  --debug_fuse set debug level for fuse
  --debug_u set debug level for u
  --readahead value readahead size, (MB) (default: 16)
  --critical check every part's etag, this option will cost cpu
  --passwd value specify access file (default: "/etc/us3fs/us3fs.conf")
  --enable_md5 enalbe md5 in http header
  --uid value specify default uid (default: 0)
  --gid value specify default gid (default: 0)

MISC
  --help, -h show help
  -f foreground
```

## Common options

* fuse settings
us3fs is based on the fuse implementation, so in addition to us3fs' own settings, it also supports fuse settings in the following format.

```bash
-o option=value
```

### Performance related

* ``parallel``: set concurrent threads, has some impact on cpu load. It is recommended to set it at 20~40 which is more reasonable
* `critical`: enable local etag checksum when writing files, will increase cpu usage by about 50% compared to not enabled.
* `readahead`: pre-reading window size, because fuse itself has the limit of read/write window, a certain pre-reading size has a significant improvement on reading performance. Recommended setting is 16~32

## Option list

| option name | description |
| -------------- | ----------------------------------------------- |
| entry_timeout | Specifies how long fuse caches the name of the file being looked up, default is 0s |
| attr_timeout | Specifies how long fuse caches file/directory attributes, defaults to 0s |
| dcache_timeout | Specifies the time for us3fs to cache file/directory attributes, default is 0s |
| async_read | Specifies that fuse reads in asynchronous mode, enabled by default.
| sync_read | Specifies that fuse reads in synchronous mode (including pre-reading), off by default |
| retry | The number of retries after a failed request, default is 5.
| parallel | Number of concurrent I/O threads, default 20 |
| debug_fuse | Specifies the user state fuse logging level to debuy, off by default.
| debug_u | Specifies the us3fs logging level as debug, default is Info level |
| readahead | Specifies the maximum pre-reading window (in MB), default is 16 |
| critical | Verify the etag of each slice when writing to a file, default is off |
| passwd | Specify the account file, default path `/etc/us3fs/us3fs.conf` |
| enable_md5 | Add md5 checksum to http request header, disabled by default |
| uid | Specify the default user to which the file belongs, default current user |
| gid | Specify the default user group the file belongs to, default current user group |
| help, h | View help |
| f | Enable foreground mode when mounting |
| update | Update the version of us3fs to /bin/us3fs |

| fuse common options list

| option name | description |
| ----------- | ---------------------------------------- |
| allow_other | Specify that the file system is accessible to all users<br>Default off |
| ro | Specify that the current file system is read-only |

## Usage examples

* **entry_timeout**, **attr_timeout**, **dcache_timeout**:

Setting `dcache_timeout` increases the time that file/directory attributes are valid in memory and enhances the usage experience. It is recommended that `entry_timeout` , `attr_timeout` be set for less than `dcache_timeout`

*Note: Turning on caching may cause inconsistency between the content of the directory read by the user and the content in the actual bucket. *

Example: ls a directory containing 10000 files timeout

```
[root@10-9-120-211 ~]# us3fs --dcache_timeout=60s --entry_timeout=60s --attr_timeout=60s testzwb /data/u2fs
[root@10-9-120-211 ~]# time ls -la /data/u2fs/test | wc -l
10003

real 0m5.964s
user 0m0.033s
sys 0m0.232s
[root@10-9-120-211 ~]#
[root@10-9-120-211 ~]#
[root@10-9-120-211 ~]# time ls -la /data/u2fs/test | wc -l
10003

real 0m0.872s
user 0m0.029s
sys 0m0.133s
```

* **async_read**, **sync_read**

Default read mode is asynchronous, synchronous read performance is poor.

Examples are as follows.

```
[root@10-9-120-211 ~]# us3fs --sync_read testzwb /data/u2fs
[root@10-9-120-211 ~]# dd if=/data/u2fs/testbig of=/dev/null bs=4M count=10
10+0 records in
10+0 records out
41943040 bytes (42 MB, 40 MiB) copied, 10.2345 s, 4.1 MB/s

[root@10-9-120-211 ~]# us3fs --async_read testzwb /data/u2fs
[root@10-9-120-211 ~]# dd if=/data/u2fs/testbig of=/dev/null bs=4M count=10
10+0 records in
10+0 records out
41943040 bytes (42 MB, 40 MiB) copied, 0.685801 s, 61.2 MB/s
```

* **parallel**

Increasing the number of concurrency can improve read and write performance, and accordingly increase system resource usage.

Examples are as follows:

```
// Default concurrency is 20
[root@10-9-120-211 ~]# us3fs testzwb /data/u2fs/
[root@10-9-120-211 ~]# dd if=/dev/zero of=/data/u2fs/testbig bs=4M count=1024
1024+0 records in
1024+0 records out
4294967296 bytes (4.3 GB, 4.0 GiB) copied, 25.5351 s, 168 MB/s

// Adjust concurrent count to 32
[root@10-9-120-211 ~]# us3fs --parallel=32 testzwb /data/u2fs/
[root@10-9-120-211 ~]# dd if=/dev/zero of=/data/u2fs/testbig bs=4M count=1024
1024+0 records in
1024+0 records out
4294967296 bytes (4.3 GB, 4.0 GiB) copied, 18.3614 s, 234 MB/s
```

* **readahead**

Adjusting the pre-reading window size has a large impact on the sequential reading of large files, recommended at 16~32MB.

Examples are as follows.

```
// Default pre-reading size 16MB
[root@10-9-120-211 ~]# dd if=/data/u2fs/testbig of=/dev/null bs=4M count=1024
1024+0 records in
1024+0 records out
4294967296 bytes (4.3 GB, 4.0 GiB) copied, 60.0498 s, 71.5 MB/s

// Resize pre-reading to 32MB
[root@10-9-120-211 ~]# us3fs --readahead=32 testzwb /data/u2fs/
[root@10-9-120-211 ~]# dd if=/data/u2fs/testbig of=/dev/null bs=4M count=1024
1024+0 records in
1024+0 records out
4294967296 bytes (4.3 GB, 4.0 GiB) copied, 37.6013 s, 114 MB/s
```
