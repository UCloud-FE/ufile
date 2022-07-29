# Quick start
- [Video instruction](#Video instruction)
- [Usage](# Usage)
- Configure access rights](# Configure access rights)
- [Set mount read-only](# Set mount read-only)
- Enable logging](#Enable logging)
- Version update](#version update)
- [Use Help](#Use Help)
- [Options list](#Options list)
- [Usage examples](#Use examples)
- [Scenario-based parameter settings](#Scenario-based parameter settings)
- [Performance-Related Parameters](#Performance-Related Parameters)
- [Small File Scenario](#Small File Scenario)
- [Local Service Support](#Local Service Support)
## Video Tutorials
Watch the following videos to quickly get started with US3FS
<video id="video" length=1000 width=800 controls="" preload="none" poster=" https://static.ucloud.cn/945f2902d9d0d5451607db50629b8dab.png  ">
<source id="mp4" src=" http://caozuozhinan.cn-bj.ufileos.com/ Screen recording 3 US3 fs.mp4 ">
</video>
## Usage
* mount
```
us3fs [global options] <bucket> <mountpoint>
```
> The parameters ``<bucket>` and ``<mountpoint>` must be the last two parameters in order, otherwise the other parameters will not take effect
* Uninstall
```
umount <mountpoint>
```
> *windows does `Ctrl+C` on cmd
-----
### Configure access rights
The default access permission for us3fs mount is the current mount user, if you need to allow other users/user groups to access the mount point, you can use the following parameters:
* `-o allow_ other`: allow any user to access the file.
* `-uid=xxx`: specify the default user
* `--gid=xxx`: Specify the default user group
The uid/gid information of the user can be obtained by the `-id` command, the example is as follows.
```bash
// mount us3fs with default user and usergroup www under ubuntu account
ubuntu:~$ id www
uid=1001(www) gid=1001(www) groups=1001(www)
ubuntu:~$ us3fs --uid=1001 --gid=1001 -o allow_ other <bucket> <mountpoint>
```
* If the following problem occurs with the mount
```bash
stderr:
/bin/fusermount: option allow_ other only allowed if 'user_ allow_ other' is set in /etc/fuse.conf
```
Add ``user_ allow_ other`` to ``/etc/fuse.conf``.
> *Only `--uid` and `--gid` are supported under windows*
### Set mount read-only
Specify `-o ro` when mounting.
> not supported under windows
### Turn on logging
* --level=info/debug/error Enables us3fs logging at the specified level
* --debug_ fuse enables user state fuse logging
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
The updated executable is placed in the `/bin/` directory
> *windows invalid*
-----
## Help for use
View the parameters supported by us3fs via `us3fs -h`
#### Linux
``bash
❯ us3fs -h
us3fs - a single posix file system based on us3
USAGE
us3fs [global options] bucket mountpoint
Version
US3FS Version: 1.6.7
Commit ID: d5569b9
Build: 2022-04-21:16:28:09
Go Version: go1.16.3 linux/amd64
FUSE
-o value Specify fuse/winfsp option
--entry_ timeout value How long to cache dentry for inode for fuse.  (default: 5m0s)
--attr_ timeout value How long to cache inode attr for fuse (default: 5m0s)
--disable_ async_ read Disable all read (even read-ahead) operations asynchronously
--wb Enable writeback mode, which is turned off by default
--max_ background value Specify the max_ background parameter of fuse kernel(>=7.13), currently fuse usespace supports up to 1024 (default: 64)
--congestion_ threshold value Specify the congestion_ threshold parameter of fuse kernel(>=7.13), currently fuse usespace supports up to 768 ( default: 48)
--async_ dio Enable the async_ dio parameter of fuse kernel, async_ dio is disabled by default
--keep_ pagecache Turn on pagecache, when the file is opened, it will be decided whether to update according to the modification time of the inode, so Please pay attention to the attr_ timeout and dcache_ timeout parameters will have a certain impact on this
OS
--dcache_ timeout value How long to cache dentry for us3fs (default: 5m0s)
--retry value Number of times to retry a failed I/O (default: 5)
--parallel value Number of parallel I/O thread (default: 32)
--debug Set debug level for fuse/winfsp
--level value Set log level: error/warn/info/debug (default: "info")
--log_ dir value Set log dir
--log_ max_ age value Set log max age (default: 72h0m0s)
--log_ rotation_ time value Set log rotation time (default: 1h0m0s)
--readahead value Readahead size.  e.g.: 1m/1k/1 (default: "0")
--etag value Check etag for part.  value is percent(0~100) (default: 50)
--passwd value specify access file (default: "/etc/us3fs/us3fs.yaml")
--enable_ md5 Enalbe md5 in http header
--uid value Specify default uid (default: 502)
--gid value Specify default gid (default: 20)
--disable_ check_ vdir disable detection of virtual directories
--update Update us3fs to /bin/us3fs
-n Doesn't check access when mount us3fs
-l Enable local cache for small file
-p value Specify local cache location (default: "/tmp/us3fs/")
--prefix value Specify bucket prefix path
--gfl Enable get_ file_ list
--direct_ read Enable cache bypass read
--perf_ dump value How long to output the performance dump (default: 1h0m0s)
--skip_ ne_ dir_ lookup Skip non-essential directory checking, such as files ending in ".log", ".png", ".jpg", etc.
--storage_ class value Storage type, including "STANDARD", "IA" (default: "STANDARD")
MISC
--help, -h show help
-f foreground
```
#### Windows
```shell
us3fs - a single posix file system based on us3
USAGE
us3fs [global options] bucket mountpoint
Version
US3FS Version: 1.6.6
Commit ID: 541a5aa
Build: 2022-01-21:11:51:10
Go Version: go1.16.3 linux/amd64
WinFSP
-o value Specify fuse/winfsp option
--dir_ info_ timeout value The expiration time of the directory information, in seconds (default: 5)
--file_ info_ timeout value File information expiration time, in seconds (default: 5)
--volume_ info_ timeout value Volume information expiration time, in seconds (default: 5)
--case_ insensitive Is case sensitive
--keep_ filecache keep filecache
OS
--dcache_ timeout value How long to cache dentry for us3fs (default: 5m0s)
--retry value Number of times to retry a failed I/O (default: 5)
--parallel value Number of parallel I/O thread (default: 32)
--debug Set debug level for fuse/winfsp
--level value Set log level: error/warn/info/debug (default: "info")
--readahead value Readahead size.  e.g.: 1m/1k/1 (default: "0")
--etag value Check etag for part.  value is percent(0~100) (default: 50)
--passwd value specify access file (default: "/etc/us3fs/us3fs.conf")
--enable_ md5 Enalbe md5 in http header
--uid value Specify default uid (default: -1)
--gid value Specify default gid (default: -1)
--disable_ check_ vdir disable detection of virtual directories
--update Update us3fs to /bin/us3fs
-n Doesn't check access when mount us3fs
-l Enable local cache for small file
-p value Specify local cache location (default: "/tmp/us3fs/")
--prefix value Specify bucket prefix path
--gfl Enable get_ file_ list
--direct_ read Enable cache bypass read
--perf_ dump value How long to output the performance dump (default: 1h0m0s)
--skip_ ne_ dir_ lookup Skip non-essential directory checking, such as files ending in ".log", ".png", ".jpg", etc.
--storage_ class value Storage type, including "STANDARD", "IA" (default: "STANDARD")
MISC
--help, -h show help
-f foreground
```
> * The main differences between Linux and Windows parameters are `FUSE` and `WinFsp`*
## List of options
#### WinFsp
| Option Name | Description |
| ------------------- | -------------------------- |
| o | Option parameters supported by WinFsp |
| dir_ info_ timeout | Timeout for directory caching, default 5s |
| file_ info_ timeout | Timeout for the file cache, default 5s |
| volume_ info_ timeout | Timeout for volume information, default 5s |
| keep_ filecache | Whether to put files into cache |
| | |
#### FUSE
| option name | description |
| -------------------- | ------------------------------------------------------------ |
| o | Option parameters supported by FUSE |
| entry_ timeout | Specifies how long fuse caches the name of the file being looked up<br/>default is 5min |
| attr_ timeout | Specifies how long fuse caches file/directory attributes<br/>default is 5min |
| disable_ async_ read | Disables fuse kernel pre-reading using asynchronous mode.  Enabled by default |
| wb | Specifies that writes use writeback mode.  Overwrite/append writes are not supported.
| max_ background | Specifies the max_ background parameter of the fuse kernel (fuse kernel version >= 7.13),<br> currently fuse usespace supports up to 1024 (default: 64), which improves the parallelism of direct io.
| congestion_ threshold | Specifies the congestion_ threshold parameter for fuse kernel (fuse kernel version >= 7.13), <br>currently fuse usespace supports up to 768 (default: 48), this parameter triggers congestion control for parallel IO |
| async_ dio | Enables the async_ dio parameter in the fuse kernel, and turns it off by default.  when this parameter is on, <br>the fuse kernel does asynchronous processing of direct io |
| keep_ pagecache | When pagecache is turned on, the file will be updated or not based on the modification time and size change of the inode, so please note that the entry_ timeout and dcache_ timeout parameters will<br>have an impact on this, making the file not aware of the modification time and size change in time.  changes |
* List of common fuse options (used with `-o`)
| option name | description |
| ----------- | ---------------------------------------- |
| allow_ other | Specifies that the file system is accessible to all users<br>Default off |
| ro | Specifies that the current file system is read-only |
**How to use**
```bash
-o option=value
```
#### OS(Object Storage)
| option name | description |
| ------------------ | ------------------------------------------------------------ |
| dcache_ timeout | The length of time the dentry cache is cached in us3fs<br/>default is 5min |
| retry | Number of retries after a failed request, default 5 |
|