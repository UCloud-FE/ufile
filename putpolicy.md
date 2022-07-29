# Upload Policy Description



US3 Upload Policy (PutPolicy) is used to upload an object while completing some specified actions that will be triggered and completed after the upload action is completed (some actions are executed before the upload is started).

APIs that can use PutPolicy include: PutFile, FinishMultipartUpload.

## US3 upload callbacks

The upload policy enables callbacks to other services (callbacks) to process the uploaded files.

The policy specifies to request a user-specified service address (currently only http is supported, and only one specified service is supported to be The policy specifies to request a user-specific service address (currently only http is supported, and only one specified service is supported to be requested) after the upload of the file is completed (with possible parameters). After getting a response from the user's server (which must be in application/json format), the return value from the user's server is passed to the user. The address of the callback service is encapsulated in json format, as follows.

```
{
"callbackUrl" : "http://test.ucloud.cn", //specify the address of the callback service
"callbackBody" : "key1=value1&key2=value2" // the parameters passed to the callback service
}
```

The authorization field Authorization section of an API request that carries an upload policy differs from one that does not carry an upload policy.

Without the upload policy, the Authorization format for uploads is :

```
Authorization: UCloud publickey:signature
```

Using the upload policy, the format is:

```
Authorization: UCloud publickey:signature:encodedPutPolicy
```

where encodedPutPolicy = base64(json\_ encode(put\_ policy))
**(Note: Please use compressed format for json format, do not carry whitespace, unless the key/value itself is a string with whitespace. is the base64 of URLSafe)**

In addition, the old way of calculating the signature string is

```
signstring = HTTP-Verb + "\n" +
     Content-MD5 + "\n" +
     Content-Type + "\n" +
     Date + "\n" +
     CanonicalizedUCloudHeaders +
     CanonicalizedResource
ðŸ˜'

When an upload request requires the execution of an upload policy, the rest of the signature string remains unchanged and the base64 string of the upload When an upload request requires the execution of an upload policy, the rest of the signature string remains unchanged and the base64 string of the upload policy needs to be appended at the end, i.e.

```
signstring\_ with\_ putpolicy = signstring + base64(json_encode(put_policy))
```

## Usage examples

If the uploaded file is: flower.jpg, the upload policy is

```
"callbackUrl" : "<http://inner.umedia.ucloud.com.cn/CreateUmediaTask>",
"callbackBody" : "url=<http://demo.ufile.ucloud.cn/flower.jpg&patten_name=mypolicy>"
```

Upload request without upload policy:

```
PUT /flower.jpg HTTP/1.1
Content-Length: 123456
Content-Type: image/jpeg
Host: test.ufile.ucloud.cn
Authorization: UCloud aGVsbHdvZGhhZGhhc2RoYWRzZGFkaHNkaGFkaGhkaGxrc2Rh:bTgzdWhkZGlsYS9kLmFkYWRhc2Ruaw==
```

Upload request with upload policy:

```
PUT /flower.jpg HTTP/1.1
Content-Length: 123456
Content-Type: image/jpeg
Host: test.ufile.ucloud.cn
Authorization: UCloud aGVsbHdvZGhhZGhhc2RoYWRzZGFkaHNkaGFkaGhkaGxrc2Rh:ZGFkLHBwMz0xZGthZGFkYXNkYQ==: XCJjYWxsYmFja1VybFwiOlwiIGh0dHA6Ly9pbm5lci51bWVkaWEudWNsb3VkLmNvbS5jbi9DcmVhdGVVbWVkaWFUYXNrXCIsXCJjYWxsYmFja0JvZHlcIjpcInVybD1odHRwOi8vZGVtby51ZmlsZS51Y2xvdWQuY24vdGVzdC5tcDQmIHBhdHRlbl9uYW1lPW15cG9saWN5XCI =

```

Note: The signature is related to the bucket, the signature in the example is for reference only.
