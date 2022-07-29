# AWS S3 Protocol Application Scenarios

## File Browser Tools

### Feature Description

S3 Browser is an easy-to-use and powerful free client for Amazon S3. It provides a simple web service interface for storing and retrieving any amount of data, from anywhere, at any time. Files in the Bucket of US3 object storage can be directly manipulated, uploaded, downloaded, deleted, etc. through relevant configurations.

### Installation and Usage

**Applicable operating system: Windows**

### Installation steps

1. Download the installation package

Download address: [http://s3browser.com](http://s3browser.com/)

2.Installer

Go to the download page, click Download S3 Browser, follow the instructions, and install it.

### How to use

1. Add users

Click the Accounts button in the upper left corner, and in the drop-down box, click Add new account

In the Add new account page, the required items are described as follows.

**Account Name:** Account name, user-defined.

**Account Type:** Account type, select S3 Compatible Storage

**REST Endpoint:** Fixed domain name, fill in the reference to support AWS S3 protocol description. For example: s3-cn-bj.ufileos.com

**Signature Version:** Signature version, select Signature V4.

**Access Key ID:** Api public key, or Token. please refer to [S3's AccessKeyID and SecretAccessKey description](/ufile/s3/s3_introduction?id=s3-'s-accesskeyid- and- secretaccesskey- descriptions).

**Secret Access Key:** API private key. Please refer to [S3's AccessKeyID and SecretAccessKey description](/ufile/s3/s3_introduction?id=s3-'s-accesskeyid- and-secretaccesskey- description) for details.

**Encrypt Access Keys with a passward:** Please do not check this box.

**Use secure transfer(SSL/TSL):** Currently only China-Beijing II, China-Hong Kong, Vietnam-Ho Chi Minh, Korea-Seoul, Brazil-Sao Paulo, USA-Los Angeles, USA-Washington support HTTPS, other regions please do not check.

The specific configuration is filled out as follows.

! [img](images/addAccount.jpg)

Click Advanced S3-compatible storage settings in the lower left corner to configure the signature version and URL style

! [img](images/addAccount1.png)

Click Close to close the current settings, click Add new account to save the configuration, and the user is successfully created.

2. Object operation

### Console function description

! [img](images/console.png)

Special Note: Currently, the slice size only supports 8M. Configure as follows: 1. Click Tools on the toolbar above, select Options in the drop-down list, choose General, and in the pop-up page, set Enable multipart uploads with size (in megabytes) to 8, as shown in the following figure.

! [img](images/console2.png)

### Frequently Asked Questions

#### 1. Uploading files over 78G, error 400.

##### Problem Cause.

The number of s3 slices is limited to 10,000, and setting the slice to 8M can only meet the upload of files below 78G. If the file size exceeds 78G, the slice size will be adjusted automatically to ensure that the number of slices is less than 10,000. At present, us3 backend s3 protocol only supports 8M slice, if the slice size is not correct, it will return 400 error.

##### Solution.

- Use the internal beta us3browser tool for uploading (s3 protocol), if you need to use it, please contact technical support for permission to use it
- Use the us3cli tool to upload (us3 protocol). Please refer to [us3cli Tool Introduction](

## Network File System S3FS

### Function description

The s3fs utility supports mounting a Bucket locally and manipulating objects in the object store directly as if it were a local file system.

### Installation and use

**Applicable operating systems Linux, MacOS**

**Applicable s3fs version: v1.83 and above**

#### Installation steps

MacOS Environment

```
brew cask install osxfuse
brew install s3fs
```

RHEL and CentOS 7 or later via EPEL.

```
sudo yum install epel-release
sudo yum install s3fs-fuse
```

Debian 9 and Ubuntu 16.04 or later

`` `
sudo apt-get install s3fs
```

CentOS 6 and below

You need to compile s3fs and install the program

#### for source code

First, you need to download the source code from `/data/s3fs` to the specified directory, for example.

```
1. cd /data
2. mkdir s3fs
3. cd s3fs
4. wget https://github.com/s3fs-fuse/s3fs-fuse/archive/v1.83.zip
```

#### install dependencies

To install dependencies on CentOS systems.

```
  sudo yum install automake gcc-c++ git libcurl-devel libxml2-devel
  fuse-devel make openssl-devel fuse unzip
```

#### compile and install s3fs

Go to the installation directory and execute the following command to compile and install.

```
1. cd /data/s3fs
2. unzip v1.83.zip
3. cd s3fs-fuse-1.83/
4. . /autogen.sh
5. . /configure
6. make
7. sudo make install
8. s3fs --version # Check the s3fs version number
```

You can check the version number of s3fs, at this point, s3fs has been successfully installed.

Remark.
During the execution of step 5, `. /configure`, the following problems may be encountered. Summarize as :

Error: configure: error: Package requirements (fuse >= 2.8.4 libcurl >= 7.0 libxml-2.0 >= 2.6 ) were not met:

Reason: fuse version is too low, in this case, you need to install fuse 2.8.4 and above manually, the installation command example is as follows.

1. yum -y remove fuse-devel #Uninstall the current version of fuse

2. wget https://.com/libfuse/libfuse/releases/download/fuse_2_9_4/fuse-2.9.4.tar.gz

3. tar -zxvf fuse-2.9.4.tar.gz

4. cd fuse-2.9.4

5. . /configure

6. make

7. make install

8. export

   PKG_CONFIG_PATH=/usr/lib/[pkgconfig:/usr/lib64/pkgconfig/:/usr/local/lib/pkgconfig](http://pkgconfig/usr/lib64/pkgconfig/:/usr/ local/lib/pkgconfig)

9. modprobe fuse #mount the fuse kernel module

10. echo "/usr/local/lib" >> /etc/[ld.so](http://ld.so/).conf

11. ldconfig #Update the dynamic link library

12. pkg-config --modversion fuse # Check the fuse version number, when you see "2.9.4", it means fuse 2.9.4 is installed successfully.

### How to use s3fs

#### Configure the key file

Create a `.passwd-s3fs` file in the `${HOME}/` directory. The file format is `[API public key:API secret key]`.

Please refer to `[S3's AccessKeyID and SecretAccessKey descriptions](/ufile/s3/s3_introduction?id=s3-'s-accesskeyid- and-secretaccesskey- descriptions) for details on how to obtain the public and private keys.

Example:

```
 [root@10-9-42-233 s3fs-fuse-1.83]# cat ~/.passwd-s3fs
 AKdDhQD4Nfyrr9nGPJ+d0iFmJGwQlgBTwxxxxxxxxxxxxxxxx:7+LPnkPdPWhX2AJ+p/B1XVFi8bbbbbbbbbbbbbbbbbbb
```

Set the file to read and write permissions.

```
 chmod 600 ${HOME}/.passwd-s3fs
```

#### Execute the mount operation

Explanation of the operation command:

- Create US3 mount file path `${LocalMountPath}`

- Get the name of the created storage (Bucket) `${UFileBucketName}`

  Note: The space name does not have a domain name extension, for example, if the US3 space name is shown as `test.cn-bj.ufileos.com`, then `${UFileBucketName}=test`

- Depending on where the US3 storage space is located, whether the local server is on the UCloud intranet, refer to [Support AWS S3 protocol description](/ufile/s3/s3_introduction)

- Execute the command.

The parameters are described as follows.

```
s3fs ${UFileBucketName} ${LocalFilePath}
-o url={UFileS3URl} -o passwd_file=~/.passwd-s3fs
-o dbglevel=info
-o curldbg,use_path_request_style,allow_other
-o retries=1 //error retry count
-o multipart_size="8" //the size of the split upload is 8MB, currently only this value is supported -o
multireq_max="8" //When the uploaded file is larger than 8MB, split upload is used, currently UFile's S3
access layer does not allow PUT of individual files larger than 8MB, so this value is recommended to be required
-f // indicates foreground execution, background execution is omitted
-o parallel_count="32" // parallel operation number, can improve the concurrent operation of the slice, suggest not to exceed 128
```

**Example:**

s3fs s3fs-test /data/vs3fs -o url=[http://internal.s3-cn-bj.ufileos.com](http://internal.s3-cn-bj.ufileos.com/) -o passwd_file=~/.passwd -s3fs -o dbglevel=info -o curldbg,use_path_request_style,allow_other -o retries=1 -o multipart_size="8" -o multireq_max="8" -o parallel_count= "32"

#### mount effect

Execute `df -h` command, you can see the s3fs program running. The effect is as follows:

! [img](images/dfh.png)

At this point, you can see that the files in the `/data/vs3fs` directory are the same as the files in the specified bucket. You can also run it through tree to see the file structure. The installation command: `yum install -y tree` works as follows.

! [img](images/yum.png)

#### file upload and download

After mounting the US3 storage and, you can use the US3 storage as if it were a local folder.

1. Copy the file to `${LocalMountPath}`, which means uploading the file. 2.
2. Copy files from `${LocalMountPath}` to other paths, i.e. download files.

**Note:**

1. Paths that do not conform to the Linux file path specification can be seen in the US3 management console, but will not be displayed under `\${LocalMountPath}` in the Fuse mount. 2.
2. Fuse will be slow to use the enumerated file list, it is recommended to use commands that specify to specific files directly, such as vim, cp, rm to specify specific files.

#### Delete files

If you delete a file from `${LocalMountPath}`, the file is also deleted from US3 storage.

#### Uninstall US3 storage

```
sudo umount ${LocalMountPath}
```

### Performance data

Write throughput around 40 MB/s Read throughput can reach 166 MB/s (related to concurrency)

## goofys

### Function description

The goofys utility, like s3fs, also supports mounting a Bucket locally and manipulating objects in the object store directly as if it were a local file system. This is better than s3fs in terms of performance.

### Installation and use

**For operating systems: Linux, MacOS**

### Steps to use

Download the executable file at

[Mac X86-64](https://github.com/ufilesdk-dev/ufile-fs/releases/download/v0.21.1/ufile-fs-mac.tar.gz) [Linux X86-64](https://github. com/ufilesdk-dev/ufile-fs/releases/download/v0.21.1/ufile-fs-linux.tar.gz)

Extract to the specified directory using the following command.

```
tar -xzvf goofys-0.21.1.tar.gz
```

By default, the public and private keys for the bucket are configured in the `$HOME/.aws/credentials` file, in the following format.

```
[default]
aws_access_key_id = TOKEN__*****9206d
aws_secret_access_key = 93614*******b1dc40
```

Execute the mount command `. /goofys --endpoint your_ufile_endpoint your_bucket your_local_mount_dir`, for example.

```
. /goofys --endpoint http://internal.s3-cn-bj.ufileos.com/suning2 . /mount_test:
```

The mount effect is shown in the figure.

! [img](images/goofys_mount.png)

To test if the mount is successful, you can copy a local file to the mount_test directory and see if it is uploaded to US3.

### Other operations (delete, upload, get, unmount)

Same as s3fs, see s3fs operations above

### Performance data

UHost VM with 4 cores and 8G, uploading files over 500MB, average speed up to 140MB/s

## US3-based FTP service

### Function description

Object Storage supports direct operation of objects and directories in Bucket via FTP protocol, including uploading files, downloading files, and deleting files (access to folders is not supported).

### Installation and usage

**Applicable operating system: Linux**

### Installation steps

#### Build environment

Use the s3fs tool to mount the Bucket locally. Refer to S3FS, US3 based **Network File System** for installation steps.

#### Install dependencies

First, check whether the local FTP service is available, execute the command `rpm -qa | grep vsftpd`, if it shows that it is not installed, then execute the following command to install FTP.

Run the following command to install vsftpd.

```
yum install -y vsftpd
```

#### start local fpd service

Execute the following command to turn on the ftp service.

```
service vsftpd start
```

### How to use S3FS

#### Add an account

Run the following command to create the ftptest user and set the specified directory.

   useradd ${username} -d {SpecifiedDirectory}

   (Command to delete a user: sudo userdel -r newuser)

2. Run the following command to change the ftptest user password.

   passwd ${username}

#### Client use

At this point, you can connect to the server from any external machine and enter your username and password to manage the bucket's files

```
ftp ${ftp_server_ip}
```

## s3cmd

### Function description

s3cmd is a free command line tool for uploading, retrieving and managing data using the S3 protocol. It is best suited for users familiar with command line programs and is widely used for batch scripts and automatic backups.

### Installation and usage

**Applicable operating systems: Linux, MacOS, Windows**

#### Installation steps
```
1. Download the installation package
https://s3tools.org/download ,here is the latest version 2.1.0 for example
2.Unpack the installation package
tar xzvf s3cmd-2.1.0.tar.gz
3.Move the path
mv s3cmd-2.1.0 /usr/local/s3cmd
4.Create soft connection
ln -s /usr/local/s3cmd/s3cmd /usr/bin/s3cmd (you can use sudo if you don't have enough privileges)
5. Execute the configuration command, fill in the necessary information (you can skip it directly, you can put it in the next step and fill it in manually)
s3cmd --configure
6. Fill in the configuration
vim ~/.s3cfg
Open the current configuration and fill in the following parameters
access_key = "TOKEN public key/API public key"
secret_key = "TOKEN private key/API private key"
host_base = "s3 protocol domain, for example: s3-cn-bj.ufileos.com"
host_bucket = "request style, e.g.: %(bucket)s.s3-cn-bj.ufileos.com"
multipart_chunk_size_mb = 8 "us3 supports s3 protocol chunk size of 8M, so only 8 can be filled here"

``''
access_key: refer to [Token public key](https://console.ucloud.cn/ufile/token)/[API public key](https://console.ucloud.cn/uapi/apikey)

secret_key: refer to [Token private key](https://console.ucloud.cn/ufile/token)/[API private key](https://console.ucloud.cn/uapi/apikey)

host_base: Reference [s3 protocol domain](https://docs.ucloud.cn/ufile/s3/s3_introduction)

#### Example configuration items

```
[default]
access_key = "TOKEN_xxxxxxxxx"
access_token =
add_encoding_exts =
add_headers =
bucket_location = US
check_ssl_certificate = True
check_ssl_hostname = True
connection_pooling = True
content_disposition =
content_type =
default_mime_type = binary/octet-stream
delay_updates = False
delete_after = False
delete_after_fetch = False
delete_removed = False
dry_run = False
enable_multipart = True
encrypt = False
expiry_date =
expiry_days =
expiry_prefix =
follow_symlinks = False
force = False
get_continue = False
gpg_passphrase =
guess_mime_type = True
host_base = s3-cn-bj.ufileos.com
host_bucket = %(bucket)s.s3-cn-bj.ufileos.com
human_readable_sizes = False
invalidate_default_index_on_cf = False
invalidate_default_index_root_on_cf = True
invalidate_on_cf = False
kms_key =
limit = -1
limitrate = 0
list_md5 = False
log_target_prefix =
long_listing = False
max_delete = -1
mime_type =
multipart_chunk_size_mb = 8
multipart_max_chunks = 10000
preserve_attrs = True
progress_meter = True
proxy_host =
proxy_port = 80
public_url_use_https = False
put_continue = False
recursive = False
recv_chunk = 65536
reduced_redundancy = False
requester_pays = False
restore_days = 1
restore_priority = Standard
secret_key = "xxxxxxxxxxxxxxxxxxxx"
send_chunk = 65536
server_side_encryption = False
signature_v2 = False
signurl_use_https = False
simpledb_host = sdb.amazonaws.com
skip_existing = False
socket_timeout = 300
stats = False
stop_on_error = False
storage_class =
throttle_max = 100
upload_id =
urlencoding_mode = normal
use_http_expect = False
use_https = False
use_mime_magic = True
verbosity = WARNING
website_index = index.html
```
How to use ####

##### 1. Upload a file
```
s3cmd put test.txt s3://bucket1
```

##### 2. Delete the file
```
s3cmd del s3://bucket1/test.txt
```

##### 3. Download the file
```
s3cmd get s3://bucket1/test.txt
```

##### 4. Copy the file
```
s3cmd cp s3://bucket1/test.txt s3://bucket2/test.txt
```
##### Other common operations
```
1. Upload a folder
s3cmd put -r . /dir s3://bucket1/dir1
2. Download a folder
s3cmd get -r s3://bucket1/dir1 . /dir1
3. Delete the folder
s3cmd del s3://bucket1/dir1
4.List the bucket list
s3cmd ls
5. List the files
s3cmd ls s3://bucket1
6. Archive file retrieval
s3cmd restore s3://bucket1
```

## rclone

### Function description

rclone is a command line program for managing files on cloud storage, supporting the s3 protocol

### Installation and use

#### Installation steps

```
curl https://rclone.org/install.sh | sudo bash

//ref https://rclone.org/install/
```

#### configuration

```
rclone config
```

#### configuration reference

```
[s3] // here you can fill in s3,ucloud and other custom names as command prefix
type = s3
provider = Other
env_auth = false
access_key_id = xxxxxxxx
secret_access_key = xxxxxxxxxxx
endpoint = http://s3-cn-bj.ufileos.com //ref
location_constraint = cn-bj
acl = private
bucket_acl = private // public/private
chunk_size = 8M // currently only 8M chunks are supported
```

access_key_id: refer to [Token public key](https://console.ucloud.cn/ufile/token)/[API public key](https://console.ucloud.cn/uapi/apikey)

secret_access_key: refer to [Token private key](https://console.ucloud.cn/ufile/token)/[API private key](https://console.ucloud.cn/uapi/apikey)

endpoint: Reference [s3 protocol domain](https://docs.ucloud.cn/ufile/s3/s3_introduction)

#### Usage

Note: The following command prefixes (the contents of the brackets in the configuration) are in remote for example, and need to be modified during use

##### 1. View all buckets

```
rclone lsd remote:
```

##### 2. List the files

```
rclone ls remote:bucket
```

##### 3. Uploading files

```
rclone copy . /test.txt remote:bucket
```

##### 4. Delete the file

```
rclone delete remote:bucket/test.txt
```

