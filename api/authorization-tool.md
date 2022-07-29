# Signature Tool

Since generating signatures is tedious, users can use signature tools to help generate signatures or as signature verification tools.

[Signature tool link](http://testsign2.cn-sh2.ufileos.com/signGenerator.html)

The signature tool currently supports.

1. file management signatures. 2.
2. private space file download address construction.

US3 also provides users to deploy their own private signature tools for the following scenarios.

1. java sdk for Andorid application development; for user image and file upload functions, which need to use public and private keys or Token for signing.
2. ios sdk for IOS application development; for user image and file uploading, you need to use public and private keys or Token for signing.

In this kind of scenario, if the public-private key or Token is built into the APP during development, and the upload and download interface is called to realize the signature and upload, the APP may be cracked, resulting in the leakage of the public-private key and Token, and the following method is recommended The following method is recommended.

1. Before user uploads, pass the upload parameters to APP's server. 2.
2. The server receives the user request, checks the login state, generates the file upload signature and returns it to APP. 3.
3. APP will use the signature returned by the server to perform file upload operation.

[Signature Service Code](https://github.com/ucloud/ufile-sdk-auth-server)