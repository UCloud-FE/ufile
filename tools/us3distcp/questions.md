

# Frequently Asked Questions

Q. When migrating backups on a large scale, how can I avoid the other operations of the hdfs cluster not being affected?

A. Allocate a task queue with fixed resources and pass it to us3distcp via `-Dmapreduce.job.queuename`. task queue resource capacity is an empirical value and needs to be considered in conjunction with the overall backup data size and characteristics.