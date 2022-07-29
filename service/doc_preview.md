# Document Preview

## Concept

The Document Preview feature supports online preview of text files, presentation files, form files, and pdf files for easy viewing of your document content.

## Caution

- File types supported for online preview

    - Supported file types for preview in pdf format: ppt, pptx, wps, doc, docx, xls, xlsx
    - Supported file types for preview in h5 format: pptx

- File Size Limit

    - Online preview of files larger than 200MB is not supported

## Call parameters

For file formats that support online preview, you can splice the parameter `x-udi-process=doc-preview&dstFormat=${format}` on the file url to preview

Example.

- For public bucket

    `http://{bucketName}.cn-bj.ufileos.com/sample.pptx?x-udi-process=doc-preview&dstFormat=h5`

- For private bucket

    `http://{bucketName}.cn-bj.ufileos.com/sample.pptx?UCloudPublicKey=${publicKey}&Signature=${signature}&Expires=1649832250&x-udi- process=doc-preview&dstFormat=h5`

> the value of dstFormat is pdf or h5

## Console preview

In the file management tab page, for files that support preview, there will be a preview button on the action column, click on it to preview the file

! [](/images/file preview1.png)
! [](/images/file-preview2.png)

## Open Locales

- North China I
- North China II
- Shanghai
- Guangzhou
