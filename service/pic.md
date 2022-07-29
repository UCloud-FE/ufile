# Image processing services
Image processing service is a massive, secure, low-cost and highly reliable image processing service provided by UCloud to the outside world.  Users upload the original images and save them in the object storage, and users can process the images at any time, any place, and on any device.
## Usage Restrictions
- Image processing services triggered only at the time of download.
- image processing unsupported locales, Thailand, Sao Paulo, Mumbai, Washington, Japan, Jakarta, Frankfurt, please contact technical support if there is an image processing request for this unsupported locale.
- only Beijing, Shanghai, Hong Kong locales support image processing requests of 10000*10000 pixels.
- image processing does not support the RANGE parameter, adding the RANGE parameter will return the entire image.
- North China II, London, Los Angeles, Nigeria do not support extranet watermark for the time being, if you need to use it, you can upload the image to US3's storage bucket first, the image url of the image watermark can use the intranet domain url.
## Function Support
Image processing service feature list.
1. [Basic image information acquisition]( https://cms-docs.ucloudadmin.com/ufile/service/pic?id= Basic picture information acquisition)
2. [EXIF information acquisition]( https://docs.ucloud.cn/ufile/service/pic?id=exif Information acquisition)
3. [Image Scaling]( https://docs.ucloud.cn/ufile/service/pic?id= Picture zoom)
4. [Image Cropping]( https://docs.ucloud.cn/ufile/service/pic?id= Image cropping)
5. [Index Cut]( https://docs.ucloud.cn/ufile/service/pic?id= Index cut)
6. [Inner cut circle]( https://docs.ucloud.cn/ufile/service/pic?id= Inscribed circle)
7. [Rounded Rectangle]( https://docs.ucloud.cn/ufile/service/pic?id= Rounded rectangle)
8. [Picture Rotation]( https://docs.ucloud.cn/ufile/service/pic?id= Picture rotation)
9. [Picture Adaptive Orientation]( https://docs.ucloud.cn/ufile/service/pic?id= Picture adaptive direction)
10. [Picture Brightness]( https://docs.ucloud.cn/ufile/service/pic?id= Picture brightness)
11. [Picture Contrast]( https://docs.ucloud.cn/ufile/service/pic?id= Picture contrast)
12. [Picture Sharpening]( https://docs.ucloud.cn/ufile/service/pic?id= Image sharpening)
13. [Picture Blur]( https://docs.ucloud.cn/ufile/service/pic?id= Picture blurred)
14. [Get Image Primary Color]( https://docs.ucloud.cn/ufile/service/pic?id= Get the main color of the picture)
15. [Watermark]( https://docs.ucloud.cn/ufile/service/pic?id= Watermark)
16. [Image Format Conversion]( https://docs.ucloud.cn/ufile/service/pic?id= Image format conversion)
17. [Picture Progressive Display]( https://docs.ucloud.cn/ufile/service/pic?id= Picture progressive display)
18. [Pipeline sequence to call multiple image processing functions]( https://docs.ucloud.cn/ufile/service/pic?id= The pipeline calls a variety of image processing functions in sequence)
## Basic image information acquisition
Basic image information includes image format, image size.
Append the imageinfo indicator (case sensitive) to the image download URL to get the basic image information in JSON format.
| Parameter Name | Value | Explanation |
| ------ | --------- | --- |
| iopcmd | imageinfo | main command |
## EXIF information acquisition
EXIF (EXchangeable Image File Format) is an exchangeable image file format specifically set for digital camera photos, obtained by appending the exif indicator (case-sensitive) to the image download URL.
Note: This method is not supported for new processed images such as thumbnails.
| Parameter Name | Value | Explanation |
| ------ | ---- | --- |
| iopcmd | exif | main command |
## Image scaling
| parameter name | value | explanation |
| ------- | ------------ | ----------------------------------------------------------------------- |
| iopcmd | thumbnail | main command |
| type | 1 | scale by percentage |
| type | 2 | width unchanged, height scaled by percentage |
| type | 3 | Scale width as a percentage with no change in height | type | 3 | Scale width as a percentage with no change in height
| type | 4 | Specify width, scale height equally |
| type | 5 | Specify the height and scale the width equally
| type | 6 | widthXheight, limited to long side, short side scaling | type | 7 | widthXheight, limited to long side, short side scaling
| type | 7 | widthXheight, define short side, scale long side adaptively | type | 8 | widthXheight, define short side, scale long side adaptively
| type | 8 | widthXheight, specify the minimum size of the height and width, scaled equally, if only the height or width is specified, the unspecified side will be cropped according to the specified value.  But beyond the specified rectangle will be centered.
| type | 9 | Scale to a larger size, and then scale to a smaller size than the specified rectangle, if the height is scaled to a larger size, then the height will be scaled equally.
| type | 10 | Scale the rectangle to a smaller size, and then scale it to be larger than the specified rectangle.
| type | 11 | Use the specified width and height to cause the image to stretch | type | 12 | Original size
| type | 12 | The original image size is smaller than the widthXheight target size, the original image is centered, and the surrounding color is filled in.
| type | 13 | Scale equivalently, with one side scaled to the target value first, and the other side centered and cropped, passing widthXheight | type
| type | 14 | Scale equally, the target will be smaller than or equal to widthXheight |
| type | 15 | Scale equivalently, the target will be equal to widthXheight.
| color | hexadecimal or color name red, etc.
| scale |max 10000 |scale percentage
| gifmode | | This parameter is only valid for gif images.  If not passed, this parameter defaults to 1.
| gifmode | 1 | Returns consecutive frames, or partial frames if all frames cannot be processed, same as 3 | gifmode | 2 | Returns the number of frames.
| gifmode | 2 | Return the first still image frame, still in gif format.
| gifmode | 3 | Returns consecutive frames within the system processing timeout, or only partial frames if all frames cannot be processed.
| gifmode | 4 | Within the system processing timeout, if all the frames cannot be processed, then return some of the jumped frames, with the frames evenly distributed.
| gifmode | 5 | If the system cannot process all frames within the processing timeout, return some of the jumped frames, with the frames evenly distributed, and fill the jumped frames with the previous frames, keeping the total number of frames before and after processing unchanged.
| gifmode | 6 | If the system can't process all the frames within the processing timeout, then return some of the jumped frames, the frames are evenly distributed, and modify the delay time of the frames so that the playback speed is the same as the original image |
## Image crop
| Parameter name | Value | Explanation |
| ------- | -------------------------------------------------------------------- | -------------------------- |
| iopcmd | crop | Main command |
| width | 0-10000 | width after crop |
| height | 0-10000 | height after crop |
| ax | 0-10000 | anchor point x coordinate |
| ay | 0-10000 | anchor y-coordinate |
| gravity | NorthWest/North/NorthEast/West Center/East/SouthWest/South/SouthEast | Position offset indicator, can be specified at the same time as the anchor point (preferred over the anchor point) |
## Index Cut
| Parameter name | Value | Explanation |
| ------- | -------------------------------------------------------------------- | -------------------------- |
| iopcmd | indexcrop | Main command |
| x | 1-Picture width | Length of each area cut in the x-direction |
| y | 1-picture height | length of each area cut in y-direction |
| i | 0-number of areas, default 0 means the first area | Select the area to return the image after cutting |
### Caution
- If the specified index value is greater than the number of areas formed after cutting, the original image will be returned.
- When both x and y are legal, the parameter of y will prevail.
## Inner cut circle
| Parameter name | Value | Explanation |
| ------- | -------------------------------------------------------------------- | -------------------------- |
| iopcmd | circle | main command |
| r | 1-4096 | Specifies the radius of the inner tangent circle |
### Caution
- If the source format of the image is PNG, WebP or BMP which supports transparent channel, the non-circular area of the image will be supplemented with transparency, if the source format of the image is JPG, the non-circular area of the image will be filled with white
- When the value of r is greater than half of the smallest side of the original picture, the value of half of the smallest side of the original picture is used to return the inner tangent circle
## Rounded Rectangle
| Parameter Name | Value | Explanation |
| ------- | -------------------------------------------------------------------- | -------------------------- |
| iopcmd | rounded-corners | Main command |
| r | 1-4096 | Cut rounded corners out of the image, specifying the radius of the rounded corners |
### Caution
- If the source format of the image is PNG, WebP or BMP, which supports transparent channel, the area outside the rounded corners of the image will be filled with transparent, if the source format of the image is JPG, the area outside the rounded corners of the image will be filled with white.
## Image rotation
| Parameter name | Value | Explanation |
| ------ | --------- | --------- |
| iopcmd | rotate | Main command |
| degree | \[0,360\] | Rotate by degrees (clockwise) |
## Adaptive orientation
| parameter name | value | explanation |
| ------- | -------------------------------------------------------------------- | -------------------------- |
| iopcmd | auto-orient | Main Command |
| value | 0, 1 | 0:Do not rotate the image with adaptive orientation, 1:Rotate the image with adaptive orientation |
## brightness
| parameter name | value | explanation |
| ------- | -------------------------------------------------------------------- | -------------------------- |
| iopcmd | bright | Main command |
| value | [-100,100] | <0:decrease the brightness of the picture, =0:don't adjust the brightness of the picture, >0:increase the brightness of the picture |
### Caution
- If the value is >=100 o