# Frequently Asked Questions

## Mount failed, "internal error" appears in the log
List service is not supported in your locale, you can specify `-gfl` to remount. (*ListObjects API locale is not supported*)

## 403 occurs when mounting

- Log ErrMsg message appears "action not allow", then check the token corresponding permission, missing and add, test after 5min
- Log ErrMsg information appears "invlid signature", first check whether the configuration file fields comply with the official documentation, followed by checking whether the token public and private keys are pasted intact

## Error is reported after the abnormal exit and mount again

1. Execute: umount <mount point>, if it fails, enter 2.
2. Execute: sudo umount <mount point>, if it fails, contact technical support

## Read and write error occurred

Report an error message such as: "Input/Output error", proceed as follows.

1. Check the system log.
   1. Path of Centos: /var/log/message
   2. Ubuntu's path: /var/log/syslog
2. If you find that there is an http status code 500 error in the logs, please contact technical support to check the reason for the Bucket request 500.

## Other users can't access after the current user is mounted

Add the parameter `-o allow_other `

## After mounting, I found that the information is not consistent with the console file
The problem is due to the default enable of entry_timeout,attr_timeout,dcache_timeout parameters and 5min expiration time, if you are sensitive to consistency, it is recommended to set these parameters to 0s to turn off

## Ways to reduce read and write latency
- Application layer system calls to write as much as possible using large IO (>=128KiB, if memory allows the use of >=1MiB to enhance the throughput effect is better), and try to maintain 4KiB alignment
- For writing large files, set the parallel parameter to increase write concurrency and improve overall write throughput
- For sequential reading of large files, it is recommended to set the parameter readahead, such as 32m
- wb enables write back mode to reduce the number of kernel and user state context switches and improve write speed, but note that files written via wb mode cannot be overwritten.
- For a large number of non-hot small files (<256KiB) sequential/random read, and large files random read scenarios, it is recommended to set the parameter direct_read
- For hot small files (<256KiB) sequential/random read scenarios, do not set the parameter direct_read
- For business needs with high IOPS, it is recommended to use asynchronous IO, and use DirectIO mode, and adjust max_background and congestion_threshold according to the actual situation.
- Enabling keep_pagecache can make use of the system VFS pagecache, which accelerates significantly for hot files, especially as web static resource storage.
- enable the parameter skip_ne_dir_lookup, which reduces the latency of lookup operations.

## High memory usage, leading to OOM
- If memory itself is tight, it is recommended that the parameters entry_timeout,attr_timeout,dcache_timeout be set to 0s
- Check if readahead is on, if it is, check if it is too big and reduce the value
- If it is not a scenario of reading large files sequentially, it is recommended to set the direct_read parameter

## The console has a corresponding directory, but the us3fs mount path does not
This proves that it is a virtual directory, so you need to execute mkdir <this directory> in the us3fs mount path to show the directory, or turn on check_virtual_dir to detect if there are files with this prefix by prefixing it with this directory to decide if it "should" be shown, but this is not recommended, it will increase the latency of us3fs. After v1.5.4 the check_virtual_dir parameter is turned on by default and cancelled, if you need to turn off this feature you can set the disable_check_vdir parameter.

## The system log shows 'too many open files'

This scenario occurs when there are a lot of random IO reads and the system limit on the number of open file descriptors (usually 1024) needs to be adjusted upwards. Check the `open files` entry with `ulimit -a` and set it to 65535 or more. Setting method:

- Single bash environment effective: `ulimit -n 65535; <us3fs mount command>`;
  - System effective: refer to [Too many open files](https://askubuntu.com/questions/1182021/too-many-open-files);
