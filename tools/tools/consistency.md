

# Consistency Matching Tool

## Tool Usage

1. Download the corresponding check toolkit at

```
linux64 http://fail-check.cn-bj.ufileos.com/check-client-linux64.zip
mac http://fail-check.cn-bj.ufileos.com/check-client-mac.zip
```

2. Unzip the toolkit, enter the directory and execute the following command to attach executable permissions to the tool.

```
chmod +x chmod.sh
. /chmod.sh
```

3. Use . /get_file_list.sh to pull the list of problematic files

```
. /get_file_list.sh <bucket name to check> <local file directory to check> <us3 directory to check>

Executing the above command produces the following files.
checkData.txt : holds a list of four suspicious files
invalid_content.txt : a list of files with inconsistent etag or inconsistent size
local_not_exists.txt: a list of files that do not exist locally relative to the us3 directory
new_files.txt : A list of files that do not exist in us3 relative to the local directory

The four cases in //checkData.txt are distinguished
[1] means us3 file does not exist
[2] indicates that the local file does not exist
[3] means the file size is inconsistent
[4] means the etag is inconsistent
```

4. If you need to re-upload the batch files with inconsistent etag and size, execute the following command

```
. /upload_invalid_content.sh <bucket to be uploaded>
``` .

5. If you need to upload a file that us3 is missing, execute the following command

```
. /upload_new_files.sh <bucket to upload>
```

6. Recheck after the upload is complete

```
. /check.sh <bucket name to be checked> <local file directory to be checked> <us3 directory to be checked
```

## checkData file content parsing

1. us3 file does not exist

```
[1],us3 file does not exist, local file name, us3 file name
```

2. local file does not exist

```
[2],local file does not exist,us3 file name
```

3. file size inconsistency

```
[3],inconsistent file size,local file name,file size,us3 file name,file size
```

4. etag inconsistency

```
[4],inconsistent etag,local filename,file etag,us3 filename,file etag
```

####