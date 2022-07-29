

# Token Management

The US3 token feature allows flexible access to storage space and file management permissions based on user needs.

A token consists of a special pair of public and private keys. It includes attributes such as the list of allowed storage spaces, the list of allowed file It includes attributes such as the list of allowed storage spaces, the list of allowed file prefixes, operation privileges, and token expiration time.

Users can apply for different tokens on demand to complete the control of different permissions.

Users can manage US3 tokens in two ways.

1. Login to the official UCloud console and go to [US3 - Token Management](https://console.ucloud.cn/ufile/token).

Create, edit, etc. by calling [US3 API](https://docs.ucloud.cn/api/ufile-api/README).

## Create/edit token

! [](/images/guide/token.png)

token name: used to identify the token, user-defined.

Valid time: the time when the token takes effect. After the first setting is done, you can reset the expiration time by editing the token operation.

Authorized storage space: one or more storage spaces can be selected, indicating that the token can only operate these storage spaces. exist more than one storage space in a region, for example, it can only select the storage space in Beijing or Shanghai, and cannot select two regions at the For example, it can only select the storage space in Beijing or Shanghai, and cannot select two regions at the same time.

Blacklist: One or more IP blacklists can be set to restrict access to the IP list in the IP blacklist.

whitelist: one or more IP whitelist can be set to authorize access to the IP list in the IP whitelist, if no whitelist is set, the default is not to whitelist IP control, only restrict access to the blacklisted IP addresses. The current UHost intranet due to the use of IPv6 network, whitelist temporarily unable to identify the virtual IP address of the intranet IPv4, so whitelist temporarily does not support control the internal access rights of the UHost host.

Authorize files: You can choose to authorize all files, or set one or more file prefixes, indicating that the token can only access the files with these prefixes.

7. Token Permissions: The operation permissions of the token are upload, download, delete, file list and image processing, which can be multi-selected or un-selected.

Advanced permission: When the set token has the upload permission, you can set the prohibit overwrite permission to prohibit users from overwriting When the set token has the upload permission, you can set the prohibit overwrite permission to prohibit users from overwriting the file with the same name when uploading.

**Example**

An example of a complete token is shown below.

```

Public key: TOKEN_da044c8a-20bc-42a1-8b04-850535c75330

Private key: 6318b15a-faf3-4577-890d-79855313dfd9

Expiration time: 2019-04-19 00:00:00

Authorized storage space: test, test1, test2

Authorized file prefixes: file_prefix, file_prefix1, file_prefix2

Access whitelist.

Access blacklist.

File management permissions: upload, download, delete, file list, image handling

Advanced permissions.

Advanced permissions.

