# API signature algorithm

US3 REST API uses a custom HTTP authentication scheme based on HMAC (Hash Message Authentication Code) keys.
To authenticate a request, you first need to merge selected elements of the request to form a string. We typically refer to this process as "signing the request" and we refer to the output HMAC algorithm as "signing" because it simulates the security properties of a real signature. Finally, you can add this signature as a parameter to the request using the syntax described in this section.

When the system receives an authenticated request, it extracts the UCloud private access key you requested and uses it in the same way to compute a It then compares the computed signature with the signature provided by the requester, If the two signatures match, then the system assumes that the requester must have access to the UCloud Private Access Key and therefore acts as the issuing authority for the principal If the two signatures do not match, then the request is discarded and an error message is returned.

Based on the above principle, we provide two authentication processes with the same principle but different details for both space management and file management interfaces. management interfaces.

## Space management signature algorithm

The public and private keys of the account can be obtained from the UCloud console in [API Products - API Keys](https://console.ucloud.cn/uapi/apikey) , click Show API Keys.

The signature of space management uses query string signature, where the client requests authorization verification by passing the signature field as a A request to create a space with a signature has the following format:

    GET /Type=public&BucketName=demobucket&PublicKey=uclouddemo@mail.com45207436768156091&Action=CreateBucket&Signature= 13f7989d4a5a8ued83c0e57ah43b3607bc506c7c HTTP/1.1

where the pseudo-code for Signature is calculated as follows.

    // Generate the query dictionary for the request
    querys = {"PublicKey" : publicKey} + {other query fields}
    //Sort the query dictionary by dictionary order (lexicographical order)
    querys = querys.sort()
    //generate signstring
    signstring = ""
    for key, value in querys {
       signstring += key + value
    }
    // Add the private key to the signature
    signstring += privateKey
    //compute the signature according to SHA1 (RFC 3174)
    signature = sha1(signstring)
    //display the signature in hexadecimal
    signature = HEX(signature)

Signatures for other space management APIs can be generated using the same calculation.

## File management signature algorithm

File management signatures are passed to the server via HTTP requests in two different ways, the authentication header and the query string request authentication alternative.

### Authentication header

This method uses the Authorization header field to pass the signature data and is placed in the message header of each HTTP request, as shown below, the authentication header has the following form.

    Authorization: UCloud UCloudPublicKey:Signature

The Signature is a hash, specifically the HMAC-SHA1 (RFC2104) of a specific element in the request, so the Signature will vary from request to request. If the Signature attached to the client request matches the Signature calculated by the server, the requestor has the access rights allowed by UCloud. following is the pseudo code for the Authorization authentication header construction.

    Authorization = "UCloud" + " " + UCloudPublicKey + ":" + Signature
    Signature = Base64( HMAC-SHA1( UCloudPrivateKey, UTF-8-Encoding-Of( StringToSign ) ) )
    StringToSign = HTTP-Verb + "\n" +
        Content-MD5 + "\n" +
        Content-Type + "\n" +
        Date + "\n" +
        CanonicalizedUCloudHeaders +
        CanonicalizedResource
    CanonicalizedUCloudHeaders = <described below>
    CanonicalizedResource = "/" + Bucket + "/" + Key

Two types of header elements are included in StringToSign.

One is the location headers, of which there are only 3, Content-MD5, Content-Type and Date, and the names of these headers are not included in StringToSign , only their values in the request, which need to be replaced by the empty string `(")` if they do not exist in the request; the other is the UCloud additional headers, starting with `X -These headers need to be added to StringToSign after constructing the CanonicalizedUCloudHeaders string as specified below .

Remark.

1. if the location header is not in the request (for example, Content-Type or Content-MD5 is optional for PUT requests and has no meaning for GET requests), The location must be replaced with the empty string "". 2.
standardbase64 is used for BASE64, not the base64 algorithm of URLSafe, as below.
3. when using a POST form upload, the Content-Type field used for the signature should be the Content-Type field in the form parameter (i.e. the mimetype of UCloudPublicKey and UCloudPublicKey should be used for the signature.
UCloudPublicKey and UCloudPrivateKey correspond to tokens created in [token management](/ufile/guide/token), which can also be accessed by users 5. Keys can be used as raw access keys.
Keys can be used as raw strings without url encoding

**Steps to calculate CanonicalizedUCloudHeaders**

For example, change `X-UCloud-Date` to `x-ucloud-date`.

2. Arrange the header sets in dictionary order according to the header names.

3. Combine header fields with the same name into a `header-name:comma-separated-value-list` pair without spaces between the values, as specified in RFC2616, section 4.2.

For example, metadata headers `x-ucloud-meta-username:fred` and `x-ucloud-meta-username:barney` can be merged into a single header `x-ucloud- meta-username:fred,barney`.

4. "Expand" long headers that span multiple lines by replacing collapsed spaces (including line breaks) with a single space (as allowed in RFC 2616, section 4.2).

Remove the spaces around the colon in the header. For example, the header `x-ucloud-meta-username:fred,barney` is replaced with `x-ucloud-meta- username:fred,barney`.

Finally, append a newline character (U+000A) to each of the normalized headers in the resulting list. build the CanonicalizedUcloudHeaders element by normalizing all the headers in this list to a single string.

**Example**

Determine the API interface to use, e.g. using the PUTFile interface, with the following pre-signature request

    PUT /demokey HTTP/1.1
    Host: demobucket.ufile.ucloud.cn
    Content-Length: 11434
    Content-Type: image/jpeg
    X-UCloud-Foo: foo
    X-UCloud-Bar: bar1
    X-UCloud-Bar: bar2

2. Splice the signature string against each parameter of the request in step1, the variables in the program take the following values (described in pseudo -code)

```
bucket = "demobucket"
key = "demokey"
http_verb = "PUT"
content_md5 = ""
content_type = "image/jpeg"
date = ""
canonicalized_ucloud_headers = "x-ucloud-foo:foo" + "\n" + "x-ucloud-bar:bar1,bar2" + \n"
canonicalized_resource = "/" + "demobucket" + "/" + "demokey"
string2sign = "PUT" + "\n"
    + "" + "\n"
    + "image/jpg" + "\n"
    + "" + "\n"
    + "x-ucloud-foo:foo" + "\n" + "x-ucloud-bar:bar1,bar2" + "\n"
    + "/demobucket/demokey"
ðŸ˜'

that is

    string2sign = "PUT\n\nimage/jpeg\n\nx-ucloud-foo:foo\nx-ucloud-bar:bar1,bar2\n/demobucket/demokey"

3. hmac-sha1 operation

Use the private key assigned to you by UCloud to do hmac-sha1 operations on the signature string

    hmacstring = hmac-sha1(privateKey, string2sign)

4. base64 operation

Do base64 operations on the generated hmacstring

    base64string = base64(hmacstring) = S5FVD2w613MKb/hisjaqHdjvn9U=

Generate the final signature format

    signature = UCloud demouser@ucloud.cn13424346821929713944:S5FVD2w613MKb/hisjaqHdjvn9U=

Generate a signed HTTP request

    PUT /demokey HTTP/1.1
    Host: demobucket.ufile.ucloud.cn
    Content-Length: 11434
    Content-Type: image/jpeg
    X-UCloud-Foo: foo
    X-UCloud-Bar: bar1
    X-UCloud-Bar: bar2
    Authorization: UCloud demouser@ucloud.cn13424346821929713944:S5FVD2w613MKb/hisjaqHdjvn9U=

### Query String Request Authentication Substitute

Instead of using the AuthorizationHTTP header, you can authenticate a specific type of request by passing the request information as a query string This is useful when allowing third-party browsers to access your US3 private space files directly, without the need for a proxy request. concept is to construct a "pre-signed" request and encode it as a URL that can be retrieved by the end-user browser. requests by specifying an expiration time.

The following is an example of a US3 REST request with an authenticated query string.

    GET /demokey.jpg?UCloudPublicKey=AKIAIOSFODNN7EXAMPLE&Expires=1141889120&Signature=vjbyPxybdZaNmGa%2ByT272YEAiv4%3D HTTP/1.1
    Host: demobucket.s3.ucloud.cn
    Date: Mon, 26 Mar 2007 19:37:58 +0000

The query string request authentication method differs slightly from the normal method only in the format of the Signature request parameter and the The following pseudo-syntax demonstrates the query string request authentication method.

```
Signature = URL-Encode( Base64( HMAC-SHA1( UCloudPrivateKey, UTF-8-Encoding-Of( StringToSign ) ) );
StringToSign = HTTP-VERB + "\n" +
    "\n" +
    "\n" +
    Expires + "\n" +
    CanonicalizedUCloudHeaders +
    CanonicalizedResource;
```

Note that the HTTPDate positional element has been replaced with Expires in StringToSign. CanonicalizedUCloudHeaders and CanonicalizedResource are CanonicalizedUCloudHeaders and CanonicalizedResource are the same.

## File ETag generation methods

The file ETag can be used to query whether a file is available for second transfer.

The pseudo-code of the specific algorithm is as follows:

    if filesize<=4MB:
        hash = base64_url_safe(blkcnt, sha(file))
    else:
        hash = base64_url_safe(blkcnt, sha(sha(blk0), sha(blk1)...))
    Field meaning
        blkcnt: the number of blocks after the file is cut into 4MB blocks, occupying 4 bytes, saved in small-end mode.
        blkN: the data of the Nth (N>=0) block.
