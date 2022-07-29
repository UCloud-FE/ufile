### v1.3.0

## Added

- The adapter adds the ability to get vmds cluster master node information from zookeeper

### v1.2.0

## Refactoring

- Us3 sdk using online version 2.6.6

### v1.1.0

## Features

- Support distcp check copy function

## Fix

- Log format printing error

### v1.0.2

## Function

- Support to turn on md5 calculation and keep its result after base64 encoding to the metadata of the file

## Fix

- When the Path of listStatus is a file, it returns an empty list, the correct semantics should be a list containing only this file
- Inaccurate byte count of write stream
- Renaming with the same file name failed
- Timeout and directory offset out-of-bounds due to ensure consistency miscalculation caused by listserv response changes
- `dead loop` problem triggered by getFilsStatus when a directory file ending with `/` is missing
- FileAlreadyExistsException thrown when the destination of rename exists, semantics should be false

## Optimization

- Notify us3vmds that logic is decoupled from the write stream

### v1.0.1

## Features

- getFileStatus
- listStatus
- mkdirs
- rename
- delete
- create
- open
- setOwner
- setPermission
- setReplication
- close
- write of OutputStream
- close of OutputStream
- read of InputStream
- seek of InputStream
- getPos of InputStream
- close of InputStream

