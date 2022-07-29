#Common commands
|Operation | command | description|
| -------------- | ------------------- | ------------------------------------------------------------ |
|Configuration management | [config] (\config) | manage the public and private keys, endpoints and other information required for uploading, including the creation, modification, deletion, update and switching of configuration items|
|Create storage space | [mb] (\mb) | create storage space|
|Delete storage space | [rb] (\rb) | delete storage space (storage space is empty)|
|View the storage space information | [stat] (#stat) | view the metadata information of the bucket|
|View the storage capacity | [du] (\du) | view the storage capacity of the bucket (standard, low frequency, archive)|
|Normal upload | [cp] (| \cp) | upload local files or directories to the storage space|
|Incremental upload | [sync] (\sync) | incremental upload directory to storage space|
|Streaming upload | [rcat] (\rcat) | upload streaming files to storage space|
|Create a directory | [mkdir] (\mkdir) | create an empty directory in the US3 storage space|
|Ordinary download | [cp] (| \cp) | download the files or directories in the storage space to the local|
|Streaming download | [cat] (\cat) | download and write the data in the storage space into the standard input|
|Copy | [cp] (| \cp) | copy files in one storage space to another storage space (the same region)|
|Move | [mv] (| #mv) | move files or directories to other directories (the same storage)|
|Delete | [rm] (| \rm) | delete files or directories in storage space|
|List | [ls] (| \ls) | list the US3 storage space list or the file list in the US3 storage space|
|Get the download URL | [sign] (\sign) | get the download link of the file in the storage space|
|View metadata | [stat] (\stat) | view metadata information of files in storage space|
|Modify metadata | [modify] (\modify) | modify the storage type, mimeType, metadata of files in the storage space|
|Archive data retrieval | [restore] (\restore) | activate archive type files to be downloadable|
|Data integrity check | [etag] (\etag) | check the file Etag of local file, standard output and US3 storage space|
|Create a token | [create token] (\create token) | create a token for operating US3|
|Delete token | [delete token] (\delete token) | delete a token used to operate US3|
|Update token | [update token] (\update token) | update a token used to operate US3|
|Description token | [describe token] (\describe token) | list and describe the token of operation US3|
|Version update | [update] (\update) | update tool version|
|Version characteristics | [version] (\version) | view tool version characteristics|
## config
The config command is used to manage configuration files.
###Command format
```
Us3cli config [--ls][--su < configuration name >][--rm < configuration name >][--cat < configuration name >][--encrypt][--ssl][--proxy [proxy address]]
[--accesskey <api/token public key >] [--secretkey <api/token private key >] [--endpoint < access domain name >]
```
###Parameter description
```
-a. --accesskey string: API key or token public key used to access US3
--Cat string: print the content of the specified configuration item
--Encrypt: whether to configure encryption
-e. --endpoint string: fixed domain name, which can be viewed on the region and domain name page
-h. --help: view the current command help
--Ls: list all current configuration items
--Proxy string: proxy address (ip:port)
--RM string: delete the specified configuration item
-s. --secretkey string: used to access the API private key or token private key of US3
--SSL: use HTTPS
--Su string: switch the specified configuration to the default configuration
```
Description of configuration file content:
|Configuration item | description | filling description|
| --------- | -------------------- | ------------------------------------------------------------ |
|AccessKey | bucket public key for authentication | [api public key]（ https://console.ucloud.cn/uapi/apikey ）, [token public key]（ https://console.ucloud.cn/ufile/token ) |
|Secretkey | bucket private key for authentication | [api private key]（ https://console.ucloud.cn/uapi/apikey ）, [token private key]（ https://console.ucloud.cn/ufile/token ) |
|Endpoint | Internet or intranet domain name | [region and domain name]（ https://docs.ucloud.cn/ufile/introduction/region ) |
|Encrypt | whether to use configuration encryption | false or true|
|Enablessl | whether to use HTTPS | false or true|
|Proxy | proxy address | "ip:port"|
Customize the format of the configuration file, and the filling instructions are the same as above:
```
accesskey: "user accesskey"
secretkey: "user secretkey"
endpoint: "ufile.cn-north-02.ucloud.cn"
encrypt: "false"
enablessl: "false"
proxy: " http://ip:port  or  https://ip:port "
```
###Use examples
1. Interactive configuration
-Create configuration item
```
#./ us3cli config
Please enter the current configuration item name: config1
Start creating a new configuration item: [config1]
Enable configuration encryption (Y or n)? n
Please enter api/token public key [current:]: xxxxxxxxxxxxxxxxxxx
Please enter the api/token private key [current:]: xxxxxxxxxxxxxxxxxxx
List of regions:
No.     RegionName      Region      
0 Beijing CN BJ
1 Shanghai II cn-sh2
2 Guangzhou CN GD
3 Hong Kong HK
4 Los Angeles US CA
5 Singapore SG
6 Jakarta IDN Jakarta
7 Taipei TW TP
8 Lagos AFR Nigeria
9 Sao Paulo bra SaoPaulo
10 Dubai UAE Dubai
11 Frankfurt Ge fra
12 Ho Chi Minh VN SNG
13 Washington US WS
14 Mumbai ind Mumbai
15 Seoul Kr Seoul
Please enter the region number: 0
Intranet and Intranet list:
No.     Network
0 Internet
1 Intranet
Please select or enter the intranet and Internet number: 0
The endpoint you selected is: [cn-bj.ufileos.com], [current:]. Please enter the enter confirmation or customize the endpoint:
Current final configuration:
ConfigName: config1
AccessKey: xxxxxxxxxxxxxxxxxxxxxx
SecretKey: xxxxxxxxxxxxxxxxxxxxxx
Endpoint: cn-bj.ufileos.com
Please check and enter enter to confirm:
Enable HTTPS (Y or n)? n
Enable agent (Y or n): n
Configuration file [config1] has been modified
Whether to use this configuration as the default configuration (the current default configuration is: < config >) (Y or n)?
```
be careful:
1. When the configuration file is created for the first time, the configuration will be automatically taken as the default configuration
2. The configuration encryption is only encrypted to the public and private keys, and the current configuration file can only be encrypted when it is first created
3. When filling in the proxy address, you only need to fill in "ip:port", and the client will supplement the required "https://" or "http://" header information according to the filled HTTPS enabling status
-List configuration items
```
./us3cli config --ls
ConfigName              ModTime                 FilePath                         Authority                 
config1  (Default)      2020-09-21 14:18:50     /root/. us3cliconfig/config1      Token       
config2                 2020-09-21 14:18:50     /root/. us3cliconfig/config2      Token     
us3cli                  2020-09-16 10:36:00     /root/. us3cliconfig/us3cli       APIKey
```
explain:
1. The default flag indicates that the configuration item is the current default configuration
2. Authority refers to permission classification, which is only used to quickly distinguish between token and API key format, and does not guarantee the accuracy of the content
-Switch configuration items
```
./us3cli config --su config2
```
-Delete configuration item
```
./us3cli config --rm config1
```
Note: the (Y or n) option rules of all the following commands are case insensitive. Entering yes or Y means confirmation, and other options mean cancellation
-Print configuration items
```us3cli config --cat config2
./us3cli config --cat config2
ConfigName：config2
AccessKey: TOKEN_ 13be86*********
SecretKey: BAtrQO8LYdgve1HS_ benbK-MXNTl3**********
Endpoint: cn-bj.ufileos.com
```
2. Non interactive configuration
```
./us3cli config config3 --accesskey TOKEN_ AAGASGAZVZV**** --secretkey USAsflmTAAF****** --endpoint cn-bj.ufileos.com
Configuration file [ config3 ] has been updated
```
3. Temporary use (effective for other orders)
-Temporarily use the configuration item config3 when uploading files
```
./us3cli cp test. txt us3://bucket1 --config config3
```
-Temporarily use the configuration file /home/ubuntu/myconfig1 when uploading files
```
./us3cli cp test. txt us3://bucket1 --config /home/ubuntu/myconfig1
```
-Use custom configuration content when uploading files
```
./us3cli cp test. txt us3://bucket1 --accesskey LTAI4G3t3BTza47xxxxxxxxxx --secretkey gznFs9daMtKmUaTq9xpxxxxxxxxxxxxx --endpoint cn-bj.ufileos.com
```
## mb
This command is used to create storage space
###Command format
```
Us3cli MB us3://< bucket name > [--acl < permission type >][--region < bucket region >][--projectid < project ID >]
```
###Parameter description
```
--AccessKey <string>: API public key or token public key used to access US3
-a. --acl <string>: permission type, which can be set to private, public, and private by default (case insensitive)
--Config <string>: the current command temporarily specifies the configuration name / configuration file path
--Endpoint <string>: fixed domain name, which can be viewed through the region and domain name page, such as: cn-bj.ufileos.com
-h. --help: instructions for the current command
--Projectid <string>: Project ID, the current bucket belongs to the project ID, and the default is default
-r. --region <string>: the region where the bucket is located, you can view the regional information, and the default region is Beijing
--Secret key <string>: used to access the API private key or token private key of US3
```
This command provides the operation of choosing between command input and interactive input. If the command input parameters, the interactive input will be automatically skipped.
###Use examples
-Hand in