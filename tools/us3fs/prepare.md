# Download and environment preparation

## Runtime environment

US3FS is implemented based on fuse under Linux and winfsp under Windows platform, your machine needs to support fuse or winfsp.

It is recommended that you run US3FS in the following environment.

* Linux
  * ceontos 7.0 and above (can be viewed by `cat /etc/redhat-release`)
  * ubuntu 16.04 and above (can be viewed via `cat /etc/issue`)
* Windows
  * Download [WinFsp Installer](https://github.com/billziss-gh/winfsp/releases/download/v1.9/winfsp-1.9.21096.msi)
  * Install according to [Official Instructions](http://www.secfs.net/winfsp/rel/)

> US3FS supports use in UCloud intranet as well as Internet environment. In intranet environment, you can use intranet domain name to improve performance and stability.

## Download link

[Linux download link](https://ufile-release.cn-bj.ufileos.com/us3fs/us3fs) or

```shell
curl -o us3fs https://ufile-release.cn-bj.ufileos.com/us3fs/us3fs
```

[Windows download link](https://ufile-release.cn-bj.ufileos.com/us3fs/us3fs.exe)

## Configure account access information

### Linux

Edit /etc/us3fs/us3fs.yaml and add the following information (if you don't have this directory you need to create it yourself).

```
bucket: testzwb
access_key: ************************************
secret_key: ************************************
endpoint: ufile.cn-north-02.ucloud.cn
hosts: []
```

*Single space after colon *

* **bucket**: the name of the storage space, needs to be the same as the name of the mounted storage space
* **access_key**: public key, supports both token secret key and api secret key mode
* **secret_key**: private key, supports both token secret key and api secret key modes
* **endpoint**: Access domain name, see [locale and domain name](https://docs.ucloud.cn/ufile/introduction/region) for details. Fill in the domain name for the geographic domain, not the specific storage space domain.
* **hosts**: Specify a list of access point IPs that will not go through DNS resolution logic to get US3 access layer IPs. If the number of specified IPs is less than 3, it will not take effect. For example: `hosts: [10.9.254.190, 117.50.123.23, 117.50.123.29, 117.50.123.8]`

The IP list specified by > hosts will be automatically marked and rejected in the 5s detection period when an abnormal (network unreachable) IP node is encountered, new requests will not be affected, but requests already initiated and links using abnormal networks, as TCP uses the fallback index retry algorithm, the default number of retries is 15, so the worst case will not be detected until about 15min. When using this parameter, it is recommended to modify the Linux parameter net.ipv4.tcp.retries2 (or modify the system file /proc/sys/net/ipv4/tcp_retries2) to 6, which can make such as network unreachability exception detectable in about 25s, thus eliminating the established abnormal links; in addition, since the current link preservation detection logic will occupy a certain amount of Please refer to the **system log too many open files** in the **scenario problem** to solve the problem.

When you need to mount multiple Buckets on one machine, you can specify the account information by `-passwd=passwd_file` (the default path is /etc/us3fs/us3fs.yaml, no need to specify).

After downloading US3FS. Use `chmod +x us3fs` to add executable permissions, or move us3fs to the /bin directory if you need to execute it directly. Example.

```shell
chmod +x us3fs
. /us3fs --passwd=passwd_file <bucket> <mountpoint>

# Move to the executable directory
mv us3fs /bin/us3fs
us3fs --passwd=passwd_file <bucket> <mountpoint>
```

### Windows

Same as linux within the configuration information, with custom configuration paths.

After downloading the executable file, move it to us3fs working directory (custom), then open the run window by pressing `windows` + `R`, enter `cmd` to enter the command line tool interface (graphical interface will be supported later), and enter the path where the executable file `us3fs.exe` is located. Example:

```shell
# Enter the drive where the executable is located, here is D drive
C:\Users\Administrator> D:
# Go to the path where the executable is located
D:\\>cd us3fs
D:\us3fs>dir
The volume in drive D is unlabeled.
The volume's serial number is 5CAF-F66B

 D:\us3fs's directory
2021/09/09 21:16 <DIR> .
2021/09/09 21:16 <DIR> .
2021/09/09 19:13 19,475,146 us3fs.exe
2021/08/26 11:29 157 us3fs.yaml
               2 files 19,475,303 bytes
               2 directories 213,768,716,288 usable bytes
# Perform a mount operation
# * Here mount to disk x and specify user with uid,gid 0, log level is debug, pre read window is 32MiB, mounted us3 bucket name is rickwu
D:\us3fs>us3fs.exe --passwd=us3fs.yaml -o debug --uid=0 --gid=0 --level=debug --readahead=32m rickwu x:

```

> *Note that currently Windows mounts are only foreground mounts *


