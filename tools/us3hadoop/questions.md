# Frequently Asked Questions

## Error on HDFS command operation on US3

If the error message is `ls: No FileSystem for scheme: us3`, please check if the configuration item `fs.us3.impl` is configured.

## Error when operating on US3

If the error message is `java.lang.RuntimeException: java.lang.ClassNotFoundException: Class cn.ucloud.us3.fs.US3FileSystem not found`, please check if the adapter jar package is placed in Hadoop looks for the CLASS_PATH path.