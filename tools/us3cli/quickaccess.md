# Quick start

- [Video Tutorial](# Introduction to use)
- [Configuration Management](# Configuration Management)
- [Create Storage](# Create Storage)
- [Upload Files or Folders](# Upload Files or Folders)
- [Download file or folder](#Download file or folder)
- [Copy file or folder](#Copy file or folder)
- [Delete file or folder](#Delete file or folder)
- [Query basic file information](#Query basic file information)
- [File Basic Information Modification](#File Basic Information Modification)

## Video instruction


Watch the following video to quickly get started with US3CLI

<video id="video" length=1000 width=800 controls="" preload="none" poster="https://static.ucloud.cn/d30b1920c4afe729edb1ead99944feaf.png ">
      <source id="mp4" src="http://caozuozhinan.cn-bj.ufileos.com/录屏1 us3 cli.mp4">
      </video>


## Configuration Management

This section introduces us3cli tool configuration methods and common usage scenarios. us3cli tool supports multiple configuration generation methods, for different configuration items, supports switching between configuration items, configuration item deletion, update, and view. For a single configuration item, it supports configuration content encryption (public and private keys), HTTPS, and proxy.

Configuration items: a configuration item represents a configuration file with different permissions, to facilitate the management of different operational rights distinction

- [Common Configuration](#Common Configuration)
  - Configuration item creation](#Configuration item creation)
  - Configuration item management](#Configuration item management)
- Temporary configuration](#Temporary configuration)

The configuration mode is distinguished according to whether the configuration file is saved or not, and is divided into common configuration and temporary configuration. The common configuration can be updated, switched, viewed, etc. The temporary configuration is entered as a parameter when other commands are run, indicating that the current configuration parameters only take effect when the current command is run.

### Common configuration

Command format.

```
us3cli config [--ls][--su <configuration name>][--rm <configuration name>][--cat <configuration name>][--encrypt][--ssl][--proxy [proxy address]]
[--accesskey <API/Token public key>][--secretkey <API/Token private key>][--endpoint <access domain>]
```

There are two common configuration creation methods: interactive configuration and one-click configuration, with the same command format and different usage

#### configuration item creation

1. Interactive configuration

Example of use.

```
#. /us3cli config
Please enter the name of the current configuration item: config1
Start creating a new configuration item: [ config1 ]
Is configuration encryption enabled (y or n) ? n
Please enter the API/Token public key [current:]: xxxxxxxxxxxxxxxxxxxxxx
Please enter the API/Token private key [current:]: xxxxxxxxxxxxxxxxxxxxxx
Region List.
No. RegionName Region
0 Beijing cn-bj
1 Shanghai II cn-sh2
2 Guangzhou cn-gd
3 Hong Kong hk
4 Los Angeles us-ca
5 Singapore sg
6 jakarta idn-jakarta
7 Taipei tw-tp
8 Lagos afr-nigeria
9 são paulo bra-saopaulo
10 Dubai uae-dubai
11 Frankfurt ge-fra
12 Ho Chi Minh City vn-sng
13 Washington us-ws
14 Mumbai ind-mumbai
15 Seoul kr-seoul
Please enter region number: 0
List of intranets and extranets.
No. Network
0 Extranet
1 Internal Network
Please select or enter the internal and external network number: 0
The endpoint you selected is: [cn-bj.ufileos.com], [current:], please enter enter to confirm or customize endponit.
Current final configuration.
ConfigName: config1
AccessKey: xxxxxxxxxxxxxxxxxxxxxxxxxx
SecretKey: xxxxxxxxxxxxxxxxxxxxxxxxxx
Endpoint: cn-bj.ufileos.com
Please check and enter Enter to confirm.
Is HTTPS enabled (y or n) ? n
Whether to enable proxy (y or n):n
The configuration file [ config1 ] has been modified
Is this configuration used as the default configuration (the current default configuration is: < config >) (y or n)?
```

2. One-click configuration

Usage example:

```
. /us3cli config config1 --accesskey TOKEN_13be86********* --secretkey BAtrQO8LYdgve1HS_benbK-MXNTl3********** --endpoint cn-bj.ufileos.com
```

#### Configuration Item Management

```
#Switch default configuration
. /us3cli config --su config1

# Delete the specified configuration (only the specified configuration item name deletion is supported, not the specified path)
. /us3cli config --rm config1

#Configuration item content view, including accesskey, secretkey, endpoint
. /us3cli config --cat config1

# View the current list of all configuration items, the default configuration will be marked with "(default)",Authority indicates whether the current configuration is Token key type or API key type (only determine the type, no permission verification)
. /us3cli config --ls
```

### Temporary configuration

Temporary configurations can be used by configuration name, configuration file path, and configuration item content, as shown in the following example:

```
#1. Specify temporary configuration by configuration name
. /us3cli ls us3://bucket1 --config config2

#2. Specify temporary configuration by config file path
. /us3cli ls us3://bucket1 --config ~/go/src/userconfig.yaml

#3. Specify the temporary configuration directly by the content of the configuration item
. /us3cli ls us3://bucket1 --accesskey "xxxxxxx" --secretkey "xxxxxxx" --endpoint "xxxxxxx"
```

Note:The current version supports custom configuration files, but only those with the same content as the one automatically generated by the tool

The content of the custom configuration file is as follows, please refer to the common commands page config command for details

```
accesskey: "user accesskey"
secretkey: "user secretkey"
endpoint: "ufile.cn-north-02.ucloud.cn"
encrypt: "false"
enablessl: "false"
proxy: "http://ip:port or https://ip:port"
```

## Create storage space

### Interactive creation

```
Command Format:
us3cli mb us3://<bucketname>

Example usage.
# . /us3cli mb us3://bucketTest

Please enter the permission type to create the bucket acl(private/public,default is private):private
List of regions.
No. RegionName Region
0 Beijing cn-bj
1 Shanghai II cn-sh2
2 Guangzhou cn-gd
3 Hong Kong hk
4 Los Angeles us-ca
5 Singapore sg
6 jakarta idn-jakarta
7 Taipei tw-tp
8 Lagos afr-nigeria
9 são paulo bra-saopaulo
10 Dubai uae-dubai
11 Frankfurt ge-fra
12 Ho Chi Minh City vn-sng
13 Washington us-ws
14 Mumbai ind-mumbai
15 Seoul kr-seoul
Please enter the region number or region code to create the bucket (default is Beijing:cn-bj):0
Region: cn-bj
The project information under the current account is as follows.
No. ProjectName ProjectId
1 Default org-orcwsj
Please enter the project number of the bucket you want to create:1
Number: 1
ProjectID: org-orcwsj
2020-11-24 17:52:56.973 INFO Make bucket [ bucketTest ] success
```

### One-click creation

```
### Command format.
us3cli mb us3://<bucketname> --projectid <projectid> --region <region> --acl <acl>

# Usage examples.
. /us3cli mb us3://buckettest --projectid org-test --region cn-bj --acl public
```

## Upload a file or folder

### Upload a single file

```
## Command format.
### Common file
us3cli cp <local filename> us3://<bucketname>/<us3key>
#Streaming files
us3cli rcat us3://<bucketname>/<us3key>

#Examples of usage.
#plain upload file without specifying any parameters
. /us3cli cp . /test.txt us3://buckettest/test.txt
#Specify storage type as IA upload (case-insensitive)
. /us3cli cp . /test.txt us3://buckettest/test.txt --storageclass IA
# Specify the number of concurrent uploads, set the number of concurrent uploads to 10 (this will only work if the file is larger than 64MB, because only large files over 64MB will be uploaded in pieces)
. /us3cli cp . /test.txt us3://buckettest/test.txt --parallel 10
# cat the local file test.txt to standard input and then upload it to us3 storage
cat test.txt | . /us3cli rcat us3://buckettest/test.txt
# Streaming upload and specifying the number of failed retries to 10, setting the concurrency to 10
cat test.txt | . /us3cli rcat us3://buckettest/test.txt --retrycount 10 --parallel 10
```

### Upload folder

```
### Command format.
us3cli cp -r <localdir> us3://<bucketname>/<us3key>

#Example of usage:
#General upload folder
. /us3cli cp -r . /testdir us3://buckettest/us3dir
#Upload a file with the suffix ".txt"
. /us3cli cp -r . /testdir us3://buckettest/us3dir --include "*txt"
#Upload files whose filenames do not contain test
. /us3cli cp -r . /testdir us3://buckettest/us3dir --exclude "*test*"
#Upload folders with integrity checks
. /us3cli cp -r . /testdir us3://buckettest/us3dir --check
```

### Incremental upload folder

Incremental upload folder: compare the local folder with the corresponding folder of us3, ignore the uploaded files, and upload the unuploaded files to the us3 folder

```
# command format:
us3cli sync <localdir> us3://<bucketname>/<us3key>

#Usage example:
# Iterate over local folders, sync with local cache, if the file is modified later than the time saved in the local cache, or not saved, upload the file, otherwise skip
. /us3cli sync . /testdir us3://buckettest/us3dir

#If the file etag is different from the etag saved in the local cache, then upload the file, otherwise skip
. /us3cli sync . /testdir us3://buckettest/us3dir --ruler etag

# iterate through the local folder, compare all the files locally and us3, if the file modification time is later than the us3 file modification time, then upload the file, otherwise skip
# If a file exists in us3's directory, but not locally, delete the file in us3 (this delete operation will ask by default, no forced delete function is currently available)
. /us3cli sync . /testdir us3://buckettest/us3dir --mode local

# iterate through the local folder, compare all files locally and us3, if the file etag is different from the etag of the same file in us3, then upload the file, otherwise skip
. /us3cli sync . /testdir us3://buckettest/us3dir --mode local --ruler etag

# incremental upload and specify storage type as low frequency
. /us3cli sync . /testdir us3://buckettest/us3dir --storageclass "IA"

# incremental upload and specify mimetype
. /us3cli sync . /testdir us3://buckettest/us3dir --mimetype "mimetype1"
```

## Download a file or folder

### Download a single file

```
## Command format.
### Normal download
us3cli cp us3://<bucketname>/<us3key> <local filename>
#Streaming download
us3cli cat us3://<bucketname>/<us3key>


#Examples of use.
#General download of a single file
. /us3cli cp us3://buckettest/test.txt . /test.txt

#Download files, specifying 8M per slice (case-insensitive and with a default size of 4M and a minimum value of 4M)
. /us3cli cp us3://buckettest/test.txt . /test.txt --partsize 4M

# Streaming file download (file will be written to standard input)
. /us3cli cat us3://buckettest/test.txt

#Streaming download and specify number of concurrent attempts, retry count is 10
. /us3cli cat us3://buckettest/test.txt --parallel 10 --retrycount 10
```

### Download folder

```
### Command format.
us3cli cp -r us3://<bucketname>/<us3key> <localdir>


#Example of usage:
#General download folder
. /us3cli cp -r us3://buckettest/us3dir . /testdir

#download folder and specify concurrent count of 20
. /us3cli cp -r us3://buckettest/us3dir . /testdir --parallel 20

#Download the folder and limit the speed to 100MB/s
. /us3cli cp -r us3://buckettest/us3dir . /testdir --speedlimit 100MB
```

## Copy a file or folder

```
## Command format
us3cli cp us3://<bucketname>/<us3key>


#Examples of usage
#Copy the file from bucket1 to bucket2 (both buckets must be in the same locale, if you need to copy data from different locales, you need to use the cross-region copy function)
. /us3cli cp us3://bucket1/test.txt us3://bucket2/test.txt

#Copy the folder from bucket1 to bucket2
. /us3cli cp -r us3://bucket1/test us3://bucket2/test

#Copy the files ending with ".txt" from the test folder in bucket1 to the test folder in bucket2
. /us3cli cp -r us3://bucket1/test us3://bucket2/test --include "*.txt"
```

## Delete a file or folder

### Delete a single file

```
## Command format.
us3cli rm us3://<bucketname>/<us3key>

#Examples of usage.
#delete file
. /us3cli rm us3://buckettest/test.txt

#Forced deletion
. /us3cli rm -f us3://buckettest/test.txt
```

### Delete the folder

```
### Command format.
us3cli rm -r us3://<bucketname>/<us3key>

#Examples of usage.
#Delete folder
. /us3cli rm -r us3://buckettest/test

#Forced deletion of the entire test folder
. /us3cli rm -r -f us3://buckettest/test

#Delete files in the test folder that do not contain test
. /us3cli rm -r -f us3://buckettest/test --exclude "*test*"

# Set the concurrency to 10 and force the current buckettest storage to be emptied
. /us3cli rm -r -f us3://buckettest --parallel 10

#Delete folders and limit the number of requests to 10 per second
. /us3cli rm -r -f us3://buckettest/test --qps 10
```

## Query basic file information

```
## Command format
us3cli stat us3://<bucketname>/<us3key>

#Usage examples
. /us3cli stat us3://buckettest/test.txt
```

## Get a list of files

```
## Command format
us3cli ls us3://<bucketname>[/us3key]

#Usage examples
. /us3cli ls us3://buckettest

#pull the files in buckettest, only 10 are displayed
. /us3cli ls us3://buckettest --limit 10

#pull files from buckettest and show them in non-directory form
#Non-directory form: all files in the directory are shown with full paths, and files in subdirectories are listed
. /us3cli ls us3://buckettest --flat

# list the files in buckettest and show if they have been fetched and when they were fetched
#Data fetched: means that the archive type data is briefly active and can be downloaded
. /us3cli ls us3://buckettest --restore
```

## File basic information modification

```
## Command format
us3cli modify us3://<bucketname>/us3key

#Usage example
#Modify the file mimetype to xxx/yyyy
. /us3cli modify us3://buckettest/test.txt --mimetype xxx/yyy

#Add metadata to the file key to "name" value to "us3cli"
. /us3cli modify us3://buckettest/test.txt --metadata name=us3cli

#Clear the metadata of the current file
. /us3cli modify us3://buckettest/test.txt --metadata "" --replace

#Modify the storage type of the file to ARCHIVE (archive type)
. /us3cli modify us3://buckettest/test.txt --storageclass ARCHIVE
```

