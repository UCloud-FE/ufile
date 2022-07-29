#Get started quickly
-[instructions for use] (# instructions for use)
-[parameter configuration] (# parameter configuration)
-[start using] (\
##Instructions for use
###Parameter configuration
There are two parameter configuration methods. Configure through interactive commands:
```shell
➜  bin  ✗ us3vmds config
Enter the listening IP address [currently: 127.0.0.1]: 192.168.185.220 / / if you need to modify the address, fill in the modified value. Otherwise, enter enter directly and use the default value of 127.0.0.1
The listening IP address is modified to 192.168.185.220
Enter the listening port [currently: 5888]: 6666 / / if you need to modify the port, fill in the modified value. Otherwise, enter enter directly, and the default value is 5888
The listening IP address is modified to 6666
Enter the api/token public key [currently:]: XXXX XXXX XXXX / / if you need to modify the public key, fill in the modified value. Otherwise, enter directly. The default is blank
Api/token public key is modified to XXXX XXXX XXXX
Enter the api/token private key [currently:]: yyyy yyyy yyyy yyyy / / if you need to modify the private key, fill in the modified value; otherwise, enter enter directly, which is blank by default
The api/token private key is modified to: yyyy yyyy yyyy yyyy
Enter the storage space bucket[currently:]: bigdata-us3 / / if you need to modify the storage space, fill in the modified value. Otherwise, enter enter directly. The default is empty
The storage space bucket is modified to bigdata-us3
Enter the access endpoint [currently:]: ufile.cn-north-02.ucloud Cn / / if you need to modify the endpoint, fill in the modified value. Otherwise, enter enter directly, which is blank by default
The access endpoint is modified to ufile.cn-north-02.ucloud.cn
Enter the log path [currently: /users/rick.wu/.us3vmds]: /var/log/us3vmds / / if you need to modify the log path, fill in the modified value. Otherwise, enter enter directly. The default is /users/rick.wu/.us3vmds
The log path is modified to: /var/log/us3vmds
Enter the log level (debug|error|info) [currently: info]: debug / / if you need to modify the log level, fill in the modified value. Otherwise, enter enter directly and use the default value info. It is recommended to always use the debug level to facilitate problem tracking
The log level is modified to: debug
Enter the hotspot path synchronization US3 cycle (unit: s) [currently: 180 s]: 3600 / / if you need to modify the hotspot path automatic synchronization cycle value, fill in the modified value. Otherwise, enter enter directly and use the default value of 180. If you only consider analyzing the data written from the big data cluster, it is recommended to increase this value to avoid the performance overhead caused by frequent synchronization
Hotspot path synchronization US3 cycle: 3600
```
Through file configuration, the path of the us3vmds configuration file is always ` $home/.us3vmds/us3vmds.yaml '. The configuration parameters are:
|Attribute key | attribute description | default value|
| :---------------: | :----------------------------------------------------------- | :------------: |
|AccessKey | access the API public key or token public key of US3 | none|
|Secret key | access the API private key or token private key of US3 | none|
|Bucket | storage space name | none|
|Endpoint | US3 intranet domain name suffix, such as ufile.cn-north-02.ucloud.cn. [refer here]（ https://docs.ucloud.cn/ufile/introduction/region ）Note that "www." should be removed| None|
|Host | IP address monitored by us3vmds | 127.0.0.1|
|Port | us3vmds listens on port | 80|
|Logdir | output log path. The log name is fixed as us3mvds.log or historical log name format, such as us3vmds-2021-02-23t02-32-21.424.log. It is recommended that the log path has a storage capacity of at least 100g | $home/ us3vmds |
|Loglevel | log is basic, and can be filled in: debug, error, info. It is recommended to set it to debug at present to facilitate follow-up problem tracking | info|
|Hotspotsyncperiod | hotspot path synchronization cycle. If you only consider analyzing the data written from the big data cluster, it is recommended to increase this value to avoid the performance overhead caused by frequent synchronization | 180 (unit: s)|
|Isbigdataonly | * * if you only consider analyzing the data written from the big data cluster, it is strongly recommended that this parameter be set to true to improve the performance of getfilestatus * *. This parameter does not display the need to manually modify the configuration file in the interactive command | false|
An example of the corresponding configuration file is as follows:
```shell
➜  bin  ✗ cat $HOME/.us3vmds/us3vmds.yaml
accesskey: TOKEN_ 7468973e-d192-4378-8253-xxxxxxxx
bucket: bigdata-us3
endpoint: internal-cn-bj.ufileos.com
host: 172.16.16.237
hotspotsyncperiod: 3600
logdir: /data/us3vmds/log
loglevel: debug
port: 80
secretkey: 9a632eb8-3938-43a0-a7e1-xxxxxxxx
isbigdataonly: true
```
You can also display configuration information through commands:
```shell
➜  bin  ✗ us3vmds config --cat
The configuration path is: /root/.us3vmds/us3vmds.yaml
The listening IP address is 172.16.16.237
Listening port: 80
Api/token public key: Token_ 7468973e-d192-4378-8253-xxxxxxxx
Api/token private key: 9a632eb8-3938-43a0-a7e1-xxxxxxxx
Storage space bucket: bigdata-us3
Visit endpoint: internal-cn-bj.ufileos.com
The log path is: /data/us3vmds/log
Log level: debug
Hotspot path synchronization US3 cycle: 3600s
```
###Start using
####Background start
```shell
➜  bin  ✗ us3vmds start
```
####Stop restart
```shell
➜  bin  ✗ us3vmds restart
```
####Stop
```shell
➜  bin  ✗ us3vmds stop
```
If debugging is required, you can also:
####Foreground start
```shell
➜  bin  ✗ us3vmds start -f
2021-02-23T20:10:19.454+0800    DEBUG   mds/cache. go:49 [uuid:70a82150-3bee-4bda-8bb6-01401a05c1e8] insert / to cach
2021-02-23T20:10:19.455+0800    INFO    mds/sync_ cache. go:418 hotspot sync period set 3600 seconds
...
```
When the us3vmds service is started, operate US3 through Hadoop and enjoy the index acceleration capability brought by us3vmds. In addition to the above functions, us3vmds has the following functions:
####Hotspot path forced synchronization
```shell
➜ bin ✗ us3vmds sync / / hibench2 / / here, us3vmds is forced to synchronize the / and /hibench2 directories
```
**Note: the synchronization path must have been loaded in us3vmds. If you are not sure, you can pull the path through Hadoop command first**