# Quick start

- [Usage](# Usage)
  - [Login](# Login)
  - [Main Screen](# Main Screen)
  - [Create directory](# Create directory)
  - [Upload files/folders](#Upload files/folders)
  - [Download](#Download)
  - [More Operations](#More Operations)
  - [History](#History)
  - [File/Folder Rename](#File/Folder Rename)
- [Account Management](#Account Management)
- [Parameter Settings](#Parameter Settings)


## Usage

Open the executable file and double click on the installed desktop application

### Login

Double click to open the icon to enter the application interface, Name option is only used for identification, Endpoint can choose external network, internal network and custom access point, you need to fill in the Bucket binding token public and private keys

The Name option is only used for identification, the Endpoint can be selected as external, internal or custom access point, you need to fill in the public and private keys of the Bucket binding token! [image](/images/us3Browser/login.png)

### Main interface

The main interface is divided into 6 main parts.
  1. directory structure, corresponding to the folder of Bucket under your account
  2. basic functions for account management and settings
  3. forward, backward, refresh shortcut keys and basic file directory operation area
  4. display area for all files and directories under the current Bucket
  5. buttons for downloading, copying download links and more operations corresponding to the current file or directory
  6. history of your own tasks, uploads and downloads
! [image](/images/us3Browser/main.jpg)

### Creating directories

Click the Create Directory button, a dialog box will pop up, enter the directory name and click OK

! [image](/images/us3Browser/createdir.jpg)

! [image](/images/us3Browser/directory.jpg)

### Uploading files/folders

Click Upload, two options will appear, File and Folder, select according to your needs, a pop-up window will appear to select local files, File can upload multiple files at the same time, Folder can only upload one at a time

Files can be uploaded at the same time, folders can only be uploaded one at a time! [image](/images/us3Browser/upload.jpg)

! [image](/images/us3Browser/upload1.jpg)

! [image](/images/us3Browser/upload2.jpg)

### Download

* Individual files or directories can be downloaded by copying the url and downloading from the website, or by directly clicking on the download and selecting the saved download path
  - Download by copying the url

! [image](/images/us3Browser/downloadurl.jpg)
  - Download and save to local

! [image](/images/us3Browser/download1.jpg)

! [image](/images/us3Browser/download2.jpg)
* Batch download, you can select multiple files or folders at once to download and save to this, select the files you want to download, click the download button, a dialog box will pop up, select the local path to store, OK

! [image](/images/us3Browser/batchdownload.png)

! [image](/images/us3Browser/batchdownload1.png)

### More actions

* More operations can perform bulk operations, mainly including delete, modify storage type and unfreeze functions, unfreeze is only valid for archive storage type
  - Delete

  ! [image](/images/us3Browser/delete1.png)

  ! [image](/images/us3Browser/delete2.png)
  - Modify the storage type

  ! [image](/images/us3Browser/modifystoragetype1.png)

  ! [image](/images/us3Browser/modifystoragetype2.png)
  - Unfreeze

 ! [image](/images/us3Browser/thaw1.png)

### History

* Mainly saves the history of user's operations (deletion, modification of storage type and unfreezing), uploads and downloads, including the total number of statistics and the number of successes, and the page display contains information about the storage bucket, local file path, target file path and progress bar display.

! [image](/images/us3Browser/history.png)

  - The task history can be cleared by the Delete All button below the action area, or deleted by a single delete button

! [image](/images/us3Browser/tasks.png)

  - Upload history, the operation area contains all pause, all resume and all delete functions, the upload process is not finished, you can pause single or all, but small files do not support pause, you can resume the upload task by all resume button, delete function as above

Delete function is the same as above! [image](/images/us3Browser/upload.png)

  - Download history, the same operation area as for upload history

! [image](/images/us3Browser/download.png)

### Rename file folders

To rename a file or directory under Bucket, click the three dots in the ribbon behind the directory or file, click Rename, and enter a new name

! [image](/images/us3Browser/rename.png)

## Account management

* Login account

! [image](/images/us3Browser/accountlogin.png)
* Modify information, you can change the login information of the current account

! [image](/images/us3Browser/modifyaccount.png)

! [image](/images/us3Browser/modifyaccount2.png)
* Log out of your account, log out of the currently logged in account

! [image](/images/us3Browser/quitaccount.png)


## Parameter settings

The settings include upload and download, list page and other settings. The upload and download settings include concurrent count, number of failed retries and slice size for large files, the list page settings mainly set the content to be displayed on the main page, and the other settings mainly set the default download save path.

! [image](/images/us3Browser/setup.png)


