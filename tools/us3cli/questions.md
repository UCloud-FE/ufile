# Frequently Asked Questions

## Error reported Open leveldb err: resource temporarily unavailable

### Cause of the problem

1. An older version of us3cli (<=v1.3.0) is used, which does not support multiple processes at the same time.

2. The same command is executed multiple times at the same time when using a newer version of us3cli.

### Solution

For reason 1, you can run the `us3cli update` command to update the tool

For reason 2, you can check if there is a running us3cli process in the background and run `kill -9 $oldus3cli process pid` to close the old us3cli process.

## Bucket operations such as stat, ls, du report error bucket not found

### Cause of the problem

If your bucket is not under the default project, it can't be found by normal operation. You need to use the --projectid option to fill in the project ID of the bucket you want to operate, i.e. projectid.

### Solution

Add --projectid <projectid> to the command you want to operate, then you can operate the bucket under the current project

```
#1. View projectid
#Method 1: mb command will show all projectid of current account when it is created
#2. Login to the console to view the project ID in the upper left corner
#Method 3: . /us3cli ls --projectid Show all projectid under current account

#2. Specify projectid to operate bucket
. /us3cli stat us3://bucketest --projectid xxxx
. /us3cli du us3://buckettest --projectid xxxx
```

## What is the difference between parallel number parallel and qps?

parallel represents the number of concurrently running tasks and does not guarantee a specific request rate.

qps stands for the number of requests per second limit, for example, if qps is 1, then the request will be limited to 1 per second.

## Upload file without specifying file key, the upload succeeds but the file is not found

### Cause of the problem

When using us3cli, you need to upload files to a folder, due to the feature that object storage can exist for files and folders with the same name, so you need to add "/" after the folder name to judge it as uploading files to the current folder, otherwise it will be recognized as ordinary upload files.

### Solution

For example, when uploading a local file test1 to the test directory of the storage bucketTest, the following method 1 will only generate a test file in the root directory, while both method 2 and method 3 can successfully upload a file to test named test1.

```
Method 1: . /us3cli cp test1 us3://bucketTest/test
Way 2: . /us3cli cp test1 us3://bucketTest/test/test1
Mode 3: . /us3cli cp test1 us3://bucketTest/test/
```

## When putting this tool into the bin directory for global use, the update command prompts a problem of not finding the file

### Cause of the problem

When updating, the tool will find the location of the executable file according to the input parameters, but there is only the command name in the parameters, not the path of the executable file, so the problem of not finding the file occurs.

### Solution

Execute `us3cli update` in the bin directory.
