

# Upload files

## Simple Upload

Simple upload refers to uploading a single file (Object) using the PutObject method in the US3 API. Simple uploads are suitable for scenarios where a Simple uploads are suitable for scenarios where a single HTTP request interaction is sufficient to complete the upload, such as uploading small files (less than 512MB).

### Usage Notes

- See [PutFile](https://docs.ucloud.cn/api/ufile-api/put_file) for more information on the API interface for simple uploads.

- For large file (> 512MB) uploads please use split uploads.

- Naming restrictions.

  * Use UTF-8 encoding.

  * Length must be between 1-1023 bytes.

  * Cannot start with a forward slash (/) or backslash (\\) character.

- To prevent unauthorized third-party uploads of data to your Bucket, US3 provides access control at the Bucket and Object levels.

- After a file is uploaded to US3, you can use the upload callback to initiate a callback request to the specified application server for the next step.

- If the upload is an image, you can also perform image processing.

## Form Upload

Form upload is done using the [PostFile](https://docs.ucloud.cn/api/ufile-api/post_file) request in the US3 API for Object uploads, which cannot exceed 1GB.

**Remarks: See PostFile for details on the API interface for form uploads.**

### Applicable scenarios

Form uploads are ideal for uploading Objects embedded in HTML pages, a common scenario is a website application, for example a job board.

| Uploading without a form | Uploading with a form |
| ---------- | --------------------------------- | -------------------------- |
| Flow comparison | Website user uploads resume. | Website users uploading resumes.
| ::: | The web server responds to the upload page. | The web server responds to the upload page. | ::: | The web server responds to the upload page.
| ::: | Resume is uploaded to the web server. | Resume is uploaded to US3. | ::: | The web server responds to the upload page.
| ::: | The web server then uploads the resume to US3. | ::: | The web server then uploads the resume to US3. | ::: | The web server then uploads the resume to US3.

In terms of process, it is easier to use form uploads, with one less step in the forwarding process.
In terms of architecture, the original uploads all went to the web server, and when the upload volume was too large, the web server needed to be expanded. With form upload, data is uploaded directly from the client to US3, and when the upload volume is too large, the pressure is on US3, which guarantees the With form upload, data is uploaded directly from the client to US3, and when the upload volume is too large, the pressure is on US3, which guarantees the quality of service.

## Piecewise upload

US3 provides the Multipart Upload function, which allows you to divide the uploaded file into multiple data blocks (also called Parts in US3) to upload them separately, and then call US3 US3 provides the Multipart Upload function, which allows you to divide the uploaded file into multiple data blocks (also called Parts in US3) to upload them separately, and then call US3's interface to combine these Parts into one Object after the upload is completed to achieve the effect of intermittent upload.


### Applicable scenarios

When using the simple upload (PutFile) function to upload a large file to US3, if a network error occurs during the upload, then the upload fails and a retry In this case, you can use a split upload to achieve the effect of a breakpoint upload.

Compared to other upload methods, split uploads are suitable for the following scenarios.

Poor network environment: such as cell phone, when there is an upload failure, you can retry the failed Part independently without re-uploading other Poor network environment: such as cell phone, when there is an upload failure, you can retry the failed Part independently without re-uploading other Parts.

2. Accelerated upload: When the local file to be uploaded to US3 is very large, you can upload multiple Parts in parallel to speed up the upload.

Streaming upload: Uploads can be started while the size of the file to be uploaded is still uncertain. This scenario is more common in industry applications such as video surveillance.


### Split upload process

Slice the files to be uploaded in a certain size.

Initialize a slice upload task [InitiateMultipartUpload](https://docs.ucloud.cn/api/ufile-api/initiate_multipart_upload).

2. Upload the partitions one by one or in parallel [UploadPart](https://docs.ucloud.cn/api/ufile-api/upload_part).

3. Finish the upload [FinishMultipartUpload](https://docs.ucloud.cn/api/ufile-api/finish_multipart_upload).


### Caution

* Except for the last part, the size of other parts cannot be smaller than 4MB, otherwise it will fail when calling FinishMultipartUpload interface.

* After the file to be uploaded is divided into parts, the file order is determined by the partNumber specified in the upload process.

* The number of concurrent uploads is not as fast as the number of concurrent uploads, but should be considered in the context of the user's own network and device load.

* By default, parts that have been uploaded but not yet called FinishMultipartUpload will not be reclaimed automatically, so if you want to terminate the By default, parts that have been uploaded but not yet called FinishMultipartUpload will not be reclaimed automatically, so if you want to terminate the upload and delete the occupied space, please call AbortMultipartUpload.

## Upload follow-up

* After the file is uploaded to US3, you can use the upload callback to initiate a callback request to the specified application server for the next step.

* If the upload is an image, you can also perform the [image processing](/ufile/service/pic) operation.

## Upload callback

US3 can provide a callback to the application server when the file upload is complete. You only need to carry the appropriate Callback parameter in the You only need to carry the appropriate callback parameter in the request sent to US3 to implement the callback.

