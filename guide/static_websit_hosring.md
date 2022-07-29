# Static web hosting
## Concept
A static website is a website where all website resources consist of static content, such as HTML, JavaScript, CSS, images and other files.  configure static website hosting policies for US3 storage buckets that have been bound to custom domains through the console
## Instructions for use
To use this feature, you must first create a custom domain name.  Then set the corresponding parameters according to the configuration interface, where Then set the corresponding parameters according to the configuration interface, where the required fields are.
- Default home page: This home page is the US3 storage bucket home page that you return to when you access the US3 storage bucket through your custom domain If you also open the subdirectory home page, the file should also exist in the subdirectory, of course you can customize the content of the file according to your own path.
- The default 404 page (but the default 404 is not filled in when the subproject home page is opened and the file 404 rule is NoSuchKey) is the error page returned when the browser accesses the file in the US3 storage bucket does not exist (404).
The US3 bucket must be `public space`, i.e. the bucket must be `public read private write`.  Also accessing through the default US3 domain will download the static site as a file.  Only access via a custom domain name bound to a US3 bucket will be rendered in the browser.
For detailed working mechanism, refer to the example.
## Example
After enabling static web hosting for Bucket, you need to upload a file with the same name as the default home page (e.g. index.html) to the target Bucket, and if the Bucket contains the directory structure prefix/, the index. html file must also be included at the directory level.  In addition, you need to upload a file with the same name as the default 404 page (e.g. error.html) to the target Bucket.  the file structure of the Bucket is shown below.
The file structure of the Bucket is shown below.
Bucket
├── index.html
├── error.html
├── us3.png
└── prefix/
└─ index.html
ðŸ ™'  ðŸ ™'
If the Bucket is bound to the custom domain name example. com, and the default home page of the configured static website is index. html and the default 404 page is error. html, then when the static website is accessed through the custom domain name, the access rules are as follows, depending on whether the subdirectory home page is opened or not.
- Not open subdirectory home page
- When you visit ` https://example.com/ and https://example.com/prefix/ `, US3 will return ` https://example.com/index.html `.
- When you visit ` https://example.com/us3.png `, the us3.png file is obtained normally.
- When you visit ` https://example.com/helloworld `, US3 will return ` https://example.com/error.html ` because helloworld does not exist.
- Opened subdirectory home page
- When you visit ` https://example.com/ `, US3 will return ` https://example.com/index.html `.
- When you visit ` https://example.com/prefix/ `, US3 will return ` https://example.com/prefix/index.html `.
- When you visit ` https://example.com/us3.png `, the us3.png file is obtained normally.
- When you visit ` https://example.com/helloworld `, because helloworld does not exist, US3 will return the corresponding information according to the file 404 rule you set.
- If the file 404 rule is set to Redirect (the default), US3 will continue to check if helloworld/index. html exists.
- If the file exists, it returns 302 and redirects the access request to ` https://example.com/helloworld/index.html `.
- If the file does not exist then a 404 is returned.
- If the file 404 rule is set to NoSuchKey, a 404 is returned directly.
- If the file 404 rule is set to Index, US3 will continue to check if helloworld/index. html exists.
- If the file exists it returns 200 and returns the contents of the file directly.
- If the file does not exist then a 404 is returned.