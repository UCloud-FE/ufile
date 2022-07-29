# History Version

## US3FS V1.6.7

### New features

* New log_dir, log_max_age, log_rotation_time parameters specify the output path of logs

### BUG FIXES

* Optimize memory usage in case of high concurrent reads and writes

## US3FS V1.6.6

### New features

* Support low frequency storage

### BUG FIXES

* Inaccurate cache elimination time caused by index ttl update exception

## US3FS V1.6.5

### BUG FIX

* The problem that tcp link is not multiplexed when using host parameter.
* The tcp link is not multiplexed when uploading large files in pieces

## US3FS V1.6.4

### BUG FIXES

* Fix write exception for proprietary cloud scenario

## US3FS V1.6.3

### BUG FIX

* Fix the problem that the backend reports an error when writing the file to submit the last tail data and cannot pass through to the user state.

## US3FS V1.6.2

### Improvements

* Exceptional runtime log capture

### BUG FIXES

* Fix the panic problem when writing files
* Default configuration path fix

## US3FS V1.6.0

### New features

* Support command line mount on windows platform
* Support access by specified ip list

### Improvements

* Cleanup logic

### BUG FIXES

* Rundown caused by concurrent reading and writing of internal data structures
* Path information loss due to forget operation triggered by fuse kernel
* Invalid -o allow_other parameter

## US3FS v1.5.5

### New features

* None

### Improvements

* Client-side http request load balancing

### BUG FIXES

* High upload pressure can cause incomplete upload slices, resulting in failure
* Null pointer causes crash

## US3FS v1.5.4

### New features

* New max_background,congestion_threshold parameters to support high concurrent io in direct mode
* New async_dio parameter to allow fuse kernel to asynchronize direct io
* New keep_pagecache parameter to cache files in vfs with constant file size and modification time

### Improvements

* Auto-completion of missing "key" format directories, directories only create files in "key/" format, and path resolution gives priority to files in "key/" format; reduce directory index creation, effectively reducing creation and query latency problems
* The parameter check_virtual_dir is enabled by default, and it is cancelled, and it needs to be closed using disable_check_vdir parameter; reduce the latency caused by the judgment of virtual directory
* New skip_ne_dir_lookup parameter to open the filename suffix filter dictionary, reduce the frequency of head operations, the current filtering support ".jpe", ".jpeg", ".png", ".gz", ".tgz", ".gz", ".tgz", ".log", ".plot", ".js", ".html" You need to make sure that there are no files with the above suffixes as directory suffixes under bucket.

### BUG FIX

* None

## US3FS v1.5.3

### New features

* New direct_read parameter supports bypassing us3fs cache to read us3 directly
* Simple performance analysis data output periodically, default 1h output once, can be adjusted by perf_dump parameter
* The mime-type of the created directory is set to "application/x-directory", which is easy to distinguish on the console

### Improvements

* entry_timeout, attr_timeout, dcache_timeout parameters are enabled by default and set to 5min
* The readahead parameter is modified to support string parsing, such as: 16m(1MiB), 8k(8KiB), 512(512B)
* Auto-completion of missing "key" format directories, reducing the number of head operations ("key" and "key/" format) to one when looking up directories
* Enable the function of parameter nodirbl by default, and cancel this parameter, replace it with semantic parameter check_virtual_dir
* Add a new filter dictionary for file name suffixes to reduce the frequency of head operations, currently only filter files with ".jpe" and ".jpeg" suffixes

### BUG FIX
* The specified uid,gid information is not initialized when assigning inode, and mtime,ctime is not initialized.
* Incomplete reading in case of too many directory members
* Allow_other parameter is not effective.

## US3FS v1.5.2

### New features

* None

### BUG FIXES

* Fixed a crash caused by reading files after a backend connection reset

## US3FS v1.5.1

### New features

* Support GetFileList to fetch files in directories; enabled by `-gfl` parameter

### BUG FIXES

* Fixed the problem of `-mv` empty directory reporting error.
* Removed the limit of pre-reading up to 64MB

## US3FS v1.5.0

### New features

* Support US3 backend dynamic sharding
* Support for specifying bucket prefix path when mounting

## US3FS v1.4.0

### New Features

* Support asynchronous upload using local cache
* Read-Your-Write support for small files

### Improvements

* Specify log level by --level

### BUG FIXES

* Fixed the problem that special characters in the file name could not be written.
* Fixed the problem that the process may be interrupted during writing.
* The problem that ls would show two directories when creating directories and files with the same name in object storage is fixed.
* Fix the problem that initialized slice upload failure would block
