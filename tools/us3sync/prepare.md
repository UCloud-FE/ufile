#Download and environment preparation
Us3sync provides two ways to deploy us3sync: general deployment and uhost image startup. Users can select "universal deployment" or "uhost image startup" us3sync service according to existing resources. The general deployment scheme is as follows:
##Operating environment
- Linux：
-CentOS 7.0 and above (can be viewed through 'cat /etc/redhat-release')
-Ubuntu 16.04 and above (can be viewed through 'cat /etc/issue')
Us3sync relies on Telnet, expect, Rsync commands to ensure that these commands are preinstalled. Using Yum source for package management, you can use the following commands to install:
```
yum install -y telnet expect rsync
```
##Download and unzip
```
wget -O US3SYNC. tgz " https://ufile-release.cn-bj.ufileos.com/US3SYNC/US3SYNC.tgz " 
tar xzf US3SYNC.tgz
cd ./US3SYNC
```
##Start the master service
```
./console. sh start
Please set the cache service listening port [9000]:
Please set the cache service password [user passwd]:
#Virtual machines are generally bound to EIPs, and it is recommended that the IP address be 0.0.0.0
Please set the web service listening address [0.0.0.0:443]:
#The internal communication address does not provide external network services. It is recommended to use the internal IP address of the machine
Please set the internal communication listening address [x.x.x.x:8080]:
Please set the number of error retry [10]:
Please set the user name used for web login [root]:
Please set the password used for web login [passwd]:
US3SYNC start success!
#View master service
./console. sh show
#End master service
./console. sh stop
#Validation
#Check whether the process starts normally
./console. sh show
```
##Add worker node
1. After the service is started, open it in the browser: https://<web service listening ip>: < web service listening port > / < br > * * Note: when using virtual machine to deploy migration services, EIP is required here, not 0.0.0.0**
2. Log in on the page and use the user name and password set at startup.
3. Add a work node. Refer to [create node interface description] (/ufile/tools/us3sync/quickaccess? Id= create node interface description). You need to provide a unique work path for each node.
Each work node needs to provide a unique work path. If the path does not exist, the corresponding directory will be automatically created< Br > * * Note: intranet IP is recommended**
##Uhost image start us3sync
Us3sync provides a virtual machine image to quickly pull up the environment. This section describes how to use the uhost image to directly start a master node running us3sync.
###Operation steps
Log in to [create host interface] on the uhost console（ https://console.ucloud.cn/uhost/uhost/create )。
After configuring various parameters in the create host interface, click the equivalent cli command line in the lower right corner, as shown:
! []( http://ufile-release.cn-bj.ufileos.com/us3sync/doc/cli_command.png )
Click the run in cloudshell button on the pop-up page:
! []( http://ufile-release.cn-bj.ufileos.com/us3sync/doc/run_cloudshell.png )
Compare the --image ID option of the command in the pop-up command line with [image startup support region] (/ufile/tools/us3sync/prepare? Id= image startup support region), and replace it with the corresponding image ID according to the available region:
! []( http://ufile-release.cn-bj.ufileos.com/us3sync/doc/replace_image_id.png )
Finally, press enter to execute the command and wait for the virtual machine to be created. You can click [uhost console]（ https://console.ucloud.cn/uhost/uhost ）View the creation and startup of virtual machines.
After the virtual machine is started, you can access the HTTPS service corresponding to the external network IP of the machine on the browser. The first login will require you to enter your user name and password (if the error server refuses to access, please wait a moment, us3sync will take a while to start):
! []( http://ufile-release.cn-bj.ufileos.com/us3sync/doc/user_passwd.png )
###Image startup support region
Image list (image support is not provided for areas not listed in the table):
|Free zone | image ID|
| :-------------------- | :------------ |
|Cn bj2 Beijing II B | uimage shd2rl|
|Cn bj2 Beijing II C | uimage-o55evb|
|Cn bj2 Beijing II d | uimage-ty5c13|
|Cn bj2 Beijing II e | uimage-fs4rf2|
|Cn-sh2 Shanghai II a | uimage-r1du3i|
|Cn-sh2 Shanghai II B | uimage slmgrf|
|Cn-sh2 Shanghai II C | uimage-b0zq42|
|Cn sh Shanghai financial cloud a | uimage-smfk3r|
|Cn GD Guangzhou B | uimage-g1oh45|
|JPN TKY Tokyo a | uimage-iushr2|
|Us CA Los Angeles a | uimage-g5y5kn|
|Bra SaoPaulo Sao Paulo a | uimage-0w3bob|
|VN SNG Ho Chi Minh a | uimage-xr05sj|
|Tw TP Taipei a | uimage vkrfzb|
|HK Hong Kong a | uimage nxwznl|
|HK Hong Kong B | uimage iayhtb|
|Ind Mumbai a | uimage-bai4bw|
|IDN Jakarta Jakarta a | uimage-04m5os|
|KR Seoul a | uimage nrgrdw|
|Rus MOSC Moscow a | uimage-4km1pq|
|AFR Nigeria a | uimage-krn5jt|
|SG Singapore a | uimage-kzjfo1|
|Th BKK Bangkok a | uimage-vxedo0|
|Th BKK Bangkok B | uimage-s0iu3x|
|Ge FRA Frankfurt a | uimage bslgo1|
|Us WS Washington a | uimage-u4ejqf|
|PH MNL Manila a a | uimage kd1jjd|
##Video reference
For deployment and use of video teaching, please refer to [[quick start]] (/ufile/tools/us3sync/quickaccess? Id= video teaching
)