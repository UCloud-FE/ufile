# Upload files to US3 on WeChat applet side

## Introduction

This article will introduce and sort out the process of uploading files to US3 using the applet native method.

## Prep

1. Create a storage bucket on the console and a token with permission to operate the bucket.
2. Get the bucket name, endpoint, and the public and private keys for the token on the console.

## Upload steps

Next we will take an image file as an example and use both PUT and POST methods to implement the file upload.

1. First we need to splice the url of the uploaded file, if you choose the PUT method to upload, the url is usually as follows.

   `https://<bucket_name>. <endpoint>/<file_name_with_prefix_on_US3>`

   Whereas with the POST method of uploading, since the file prefix (prefix) and file name on US3 are passed through the POST form parameters, we do not need to write these two parts in the url.

   `https://<bucket_name>. <endpoint>/`

2. The interface for uploading files requires us to calculate a signature for the http request using the public and private keys, so that the server can check whether the public and private keys used in the request have corresponding permissions. For the calculation method, please refer to [API Signature Algorithm](https://docs.ucloud.cn/ufile/api/authorization?id=%e6%96%87%e4%bb%b6%e7%ae%a1%e7%90%86%e7%ad%be%e5%90%8d%e7%ae% 97%e6%b3%95). Here is a js code demo to calculate the signature.

   ```javascript
    const sign = (method, publicKey, privateKey, md5, contentType, date, bucketName, fileName) => {
    const CryptoJS = require("crypto-js") // here the crypto-js encryption algorithm library is used, the installation method will be explained later
    const CanonicalizedResource = `/${bucketName}/${fileName}`
     const StringToSign = method + "\n"
                          + md5 + "\n"
                          + contentType + "\n"
                          + date + "\n"
                          + CanonicalizedResource // here md5 and date are optional, contentType is optional for PUT requests and mandatory for POST requests
     let Signature = CryptoJS.HmacSHA1(StringToSign, privateKey)
     Signature = CryptoJS.enc.Base64.stringify(Signature)
     const Authorization = "UCloud" + " " + publicKey + ":" + Signature
     return Authorization
   }
   ```

   The crypto-js in the above demo is a commonly used javaScript cryptographic algorithm library, which we recommend you to install using npm as follows.

   1. Install [nodejs](https://nodejs.org/en/) on the development machine of the applet, and try to run `npm` in the command line interface after completion to verify the installation and configuration is successful.
   2. Run `npm init` in the applet's heel directory to initialize an npm project. 3.
   3. Run the `npm i crypto-js` command to install crypto-js. See also [the official npm page](https://www.npmjs.com/package/crypto-js)
   4. Finally, use the applet to build the npm package, please refer to [build npm](https://developers.weixin.qq.com/miniprogram/dev/devtools/npm.html#_2-%E6%9E%84%E5%BB%BA-npm)

3. Next, two demos are given to show how to use the PUT and POST methods to upload files.

### PUT method

Reference document [Uploading files using the PUT method](https://docs.ucloud.cn/api/ufile-api/put_file)

```javascript
function uploadByPut(){
  const publicKey = "xxxxx-xxxxx-xxxx-xxxxx-xxxxx"
  const privateKey = "xxxxx-xxx-xxxx-xxxx-xxxxx"
  const fileName = "filename"
  const bucketName = "bucketname"
  wx.chooseImage({
     count: 1, // set the maximum number of images to 1
     sizeType: ['original', 'compressed'], // the size of the selected image
     sourceType: ['album', 'camera'], // the source of the selected image
     success(res) {
       const image = wx.getFileSystemManager().readFileSync(res.tempFilePaths[0])
       // Since this code is only a demo, the md5 and date parameters are set to empty strings, which are recommended to be filled in the official applet development
       const auth = sign("PUT", publicKey, privateKey, "", "image/jpeg", "", bucketName, fileName)
       wx.request({
         url: `http://${bucketName}.cn-bj.ufileos.com/${fileName}`,
         method: "PUT",
        header: {
          'Authorization':auth,
              'content-type': 'image/jpeg'
          },
          data: image,
          success: function (reg) {
            console.log(reg)
          }
        })
       }
    })
  }
```



### POST method

Here we use the applet's native wx.uploadFile function to implement the upload. The method uses the content-type` as `multipart/form-data, and the interface called is also the US3 interface for POST uploads, see [Uploading files using POST forms](https://docs.ucloud.cn/api/ufile-api/post_file)

```javascript
function uploadByUplodaFile(){
  const publicKey = "xxxx-xxxxx-xxxx-xxxxx-xxxxx"
  const privateKey = "xxxxx-xxx-xxxx-xxxx-xxxxx"
  const fileName = "filename"
  const bucketName = "bucketname"
  wx.chooseImage({
    count: 1, // set the maximum number of images to 1
    sizeType: ['original', 'compressed'], // the size of the selected image
    sourceType: ['album', 'camera'], // the source of the selected image
        success (res) {
          const tempFilePaths = res.tempFilePaths
          //Note that the signature method here is POST, and the content-type used for the signature calculation is still the mime-type of the file itself, i.e. "image/jpeg"
          const auth = sign("POST", publicKey, privateKey, "", "image/jpeg", "", bucketName, fileName)
          wx.uploadFile({
            url: `https://${bucketName}.cn-bj.ufileos.com/`,
            filePath: tempFilePaths[0],
            name: 'file',
            // Here FormData is the form parameter, where you specify the name of the file to be uploaded, and the authentication signature
            formData: {
              'FileName': fileName,
              'Authorization': auth
            },
            success (res){
              console.log(res)
            }
          })
        }
    })
}
```

Then if the situation does not allow using WeChat's native file upload method, then it is also possible to upload by splicing multipart/form-data. This way will be a bit more troublesome, so if there is no special need, it is still recommended to use the native method for uploading.

```javascript
function uploadByPost(){
  const publicKey = "xxxx-xxxxx-xxxx-xxxxx-xxxxx"
  const privateKey = "xxxxx-xxx-xxxx-xxxx-xxxxx"
  const fileName = "filename"
  const bucketName = "bucketname"
  // boundary can be customized, here use the boundary string used in the official US3 documentation example
  const boundary = "----UCloudPOSTFormBoundary"
  // convert string type data to binary array
  const strToBinary = (str) => {
    let res = []
    for (var i = 0; i < str.length; i++) {
      res.push(str.charCodeAt(i));
      }
    return res
  }
wx.chooseImage({
     count: 1, // set the maximum number of images to 1
     sizeType: ['original', 'compressed'], // the size of the selected image
     sourceType: ['album', 'camera'], // the source of the selected image
     success(res) {
       //read out the contents of the file and convert it to a Uint8Array
       const image = new Uint8Array(wx.getFileSystemManager().readFileSync(res.tempFilePaths[0]))
       // The signature rules here are the same as for native requests
       const auth = sign("POST", publicKey, privateKey, "", "image/jpeg", "", bucketName, fileName)
       // Splice the first half of the request body, visible and corresponding to the FormData content in the native request.
       //need to convert this part to a binary array as well
       const start = strToBinary(
                      `--${boundary}` + '\r\n' +
                      'Content-Disposition: form-data; name="FileName"' + '\r\n' +
                      '\r\n' + '\r\n' +
                      fileName + '\r\n' +
                      `--${boundary}` + '\r\n' +
                      'Content-Disposition: form-data; name="Authorization"' + '\r\n' +
                      '\r\n' +
                      auth + '\r\n' +
                      `--${boundary}` + '\r\n' +
                      'Content-Disposition: form-data; name="file"; filename="MyFilename.jpg"' + '\r\n' +
                      'Content-Type: image/jpeg' + '\r\n' +
                      '\r\n')
       //end of request body, converted to binary array
       const end = strToBinary('\r\n' + `--${boundary}--`)
       // Splice together the first half, the data of the file, and the end to form a binary array of the entire request
       const request = start.concat(Array.prototype.slice.call(image), end);
       // convert the entire reqeust into a Uint8Array and get the ArrayBuffer object with the .buffer parameter, the wx.request function finally converts the request into the corresponding string
       const data = new Uint8Array(totalArray).buffer
       wx.request({
          url: `https://${bucketName}.cn-bj.ufileos.com/`,
           method: `POST`,
        header: {
          'content-type':`multipart/form-data; boundary=${boundary}`,
        },
        data: data,
        success: function (reg) {
          console.log(reg)
        }
      })
     }
  })
}
```