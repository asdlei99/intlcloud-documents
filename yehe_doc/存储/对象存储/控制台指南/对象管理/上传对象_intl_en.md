## Overview

You can upload objects to a bucket through the **File List** page in the COS Console. For more information on objects, see [Object Overview](https://intl.cloud.tencent.com/document/product/436/13324).

> ?
> - Currently, INTELLIGENT TIERING storage class is only available in Beijing, Shanghai, Guangzhou, and Chongqing regions. To upload an object to this storage class, please [enable INTELLIGENT TIERING configuration](https://intl.cloud.tencent.com/document/product/436/38306) on the bucket.
> - DEEP ARCHIVE storage class is only available in Beijing, Shanghai, Guangzhou, and Chengdu regions.

## Prerequisites

Before uploading an object, make sure that you have already created a bucket. If no bucket has been created, see [Creating Buckets](https://intl.cloud.tencent.com/document/product/436/13309).

## Directions

#### 1. Entering the file list

 Log in to the [COS Console](https://console.cloud.tencent.com/cos5), and click **Bucket List**. Select the bucket to which you want to upload an object, and enter the **File List** page. Click **Upload Files** as shown below.
![](https://main.qcloudimg.com/raw/1f499717168f5559337899f635e19a13.png)

#### 2. Selecting object(s) to be uploaded

In the **Upload Files** page, click **Select Files** or **Select Folders** to select a single or multiple local files/folders. Click **Upload**, or click **Next** to set object attributes before clicking **Upload** (see Step 3).
![](https://main.qcloudimg.com/raw/8489d2bae2ba778bac87386093cd51e7.png)

#### 3. Setting object attributes (optional)

For the file(s) to upload, you may choose to configure fields as shown below.

- **Storage Class**: select the storage class for your object as needed. This field is set to `STANDARD` by default. For more information, see [here](https://intl.cloud.tencent.com/document/product/436/6222).

- **Access Permissions**: select the access permission for your object as needed. This field is set to `Inherit` by default (inheriting permissions from the bucket). For more information, see [Basic Concepts of Access Control](https://intl.cloud.tencent.com/document/product/436/30581).
- **Server-Side Encryption**: configure server-side encryption for the object you want to upload. COS will automatically encrypt your data as it is written and decrypt it when you access it. Currently, COS offers two encryption types: SSE-COS, and SSE-KMS (only available in Beijing, Shanghai, and Guangzhou regions). For more information, see [Server-side Encryption Overview](https://intl.cloud.tencent.com/document/product/436/18145).
-**Object Tag**: the object tag is composed of a tag key, (=), and a tag value, such as `group = IT`. You can set, query, and delete tags for specific object.
- **Metadata**: object metadata, or HTTP header, is a string sent by the server over HTTP before it sends HTML data to the browser. By modifying HTTP headers, you can modify how the webpage responds as well as certain configurations, such as caching time. Modifying an object's HTTP headers does not modify the object itself. For more information, see [Custom Headers](https://intl.cloud.tencent.com/document/product/436/13361).
  ![](https://main.qcloudimg.com/raw/0b7c7b6297a1098fe43074f85a138fbf.png)

> ?Since some browsers do not support uploading multiple files, we recommended using mainstream browsers such as IE10 or above, Firefox, or Chrome.

#### 4. Verifying upload completion

After clicking **Upload**, you can check the upload progress in **Task Completed** in the top right of the page. Once the upload is complete, the uploaded object(s) will appear in the **File List**.
![](https://main.qcloudimg.com/raw/eab5784b108fc096dbe317fed25f7925.png)

> ?The upload progress in the figure indicates the number of tasks created by the current upload operation. For example, if all 10 files are uploaded successfully, the task progress will display "succeeded: 10, failed: 0, paused: 0)".
