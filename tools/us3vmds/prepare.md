#Download and environment preparation
##Download and install
###Get executable
>Us3vmds is provided through binary executable files, and the file format is us3vmds-${version}-${os};
####Public beta version
- [us3vmds-v0.0.1-linux]( http://us3-release.cn-bj.ufileos.com/us3-bigdata/us3-vmds/v0.0.1/us3vmds-v0.0.1-linux )
- [us3vmds-v0.0.1-macOS]( http://us3-release.cn-bj.ufileos.com/us3-bigdata/us3-vmds/v0.0.1/us3vmds-v0.0.1-mac )
- [us3vmds-v0.1.0-linux]( http://us3-release.cn-bj.ufileos.com/us3-bigdata/us3-vmds/v0.1.0/us3vmds-v0.1.0-linux )
- [us3vmds-v0.1.0-macOS]( http://us3-release.cn-bj.ufileos.com/us3-bigdata/us3-vmds/v0.1.0/us3vmds-v0.1.0-mac )
####Official version
>To be published
**Note: the corresponding US3 adaptation version needs > =v1.0.2**
###Install executable
1. Download the executable file and move it to the /usr/bin directory, such as' MV us3vmds-v0.0.1 /usr/bin ';
2. Establish soft links for the latest us3vmds, such as' ln -s /usr/bin/us3vmds-v0.0.1 /usr/bin/us3vmds';
3. For the configuration parameters of us3vmds, refer to the following [instructions];
4. For the configuration parameters of US3 adapter, refer to the document [us3hadoop big data adapter tool]（ https://docs.ucloud.cn/ufile/tools/us3hadoop/quickaccess?id=%e5%8f%82%e6%95%b0%e8%af%b4%e6%98%8e ）Configuration description of parameter items' fs.us3.metadata.use 'and' fs.us3.metadata.host 'in;
**Suggestion: select namenode node for deployment node**
##High availability cluster deployment
**Note: the corresponding US3 adapter version needs to be > =v1.3.0, and the version of VMDS itself needs to be > =v0.1.0. In addition, because the container needs to go to zookeeper to obtain VMDS node information, high availability deployment will cause a certain performance loss, which is related to the number of containers and has little to do with the amount of data. Usually, the performance degradation is at the minute level**
###Steps:
1. According to [above]（ https://docs.ucloud.cn/ufile/tools/us3vmds/prepare?id=%e4%b8%8b%e8%bd%bd%e4%b8%8e%e5%ae%89%e8%a3%85 ）Configure the corresponding jar package and the corresponding core site XML configuration file. By default, the us3hadoop big data adapter will find the VMDS service node information according to the following priorities:
>1. The fs.us3.metadata.host parameter configured in the core-site.xml file directly uses the standalone single node VMDS service.
>
>2. Get the zookeeper information according to fs.us3.metadata.zookeeper.addrs configured in the core-site.xml file, and get the VMDS service master node information from zookeeper.
>3. According to the ha.zookeeper.quorum parameter in the core-site.xml file, use the zookeeper built-in in Hadoop, and obtain the VMDS service master node information from it.
Here we directly use the zookeeper that comes with Hadoop.
2. Start the VMDS service. Add the -z option to the startup parameters and attach the host:port information of zookeeper (if you do not provide port, the default zookeeper port 2181 will be used). If you have more than one zookeeper, you need to reuse the -z option. For example:
```shell
./us3vmds- start -z 10.0.0.1 -z 10.0.0.2 -z 10.0.0.3
```
The zoomeeper node information here can be used in the core-site.xml file ha.zoomeeper The address provided by the quorum parameter. Note that if you do not provide the -z option, zookeeper will use single node mode.
3. Here, the highly available VMDS service cluster is started. You can use Hadoop FS related commands to test.