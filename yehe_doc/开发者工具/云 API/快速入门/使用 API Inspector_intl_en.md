API Inspector is a new feature of TencentCloud API. It enables you to view the calls of APIs associated with each operation performed in the console. In addition, it can automatically generate API code snippets in different languages and redirect to API Explorer for online debugging. 

>!
>- API Inspector displays only the information of public [3.0 TencentCloud APIs](https://intl.cloud.tencent.com/document/api).
> - API Inspector is currently in beta test at the console and user levels.
> For the console level, this feature is currently available only in the menus of [instance](https://console.cloud.tencent.com/cvm/instance/index?rid=1), [CDH](https://console.cloud.tencent.com/cvm/cdh), [placement group](https://console.cloud.tencent.com/cvm/ps), [AS](https://console.cloud.tencent.com/autoscaling/group), [SSH key](https://console.cloud.tencent.com/cvm/sshkey), and [recycle bin](https://console.cloud.tencent.com/cvm/recycler/cvm?rid=1) in the CVM Console.
> - If you refresh the current page of the browser, the call records before the refresh will be purged.

## Benefits

API Inspector and API Explorer constitute an integrated solution for TencentCloud API users to learn and debug APIs, with the following features:
- **Automatic recording**: if you want to understand the APIs behind a specific feature, you can use API Inspector to get the call information of related APIs when using the feature in the console. For more information, please see [Automatic API call recording](#AutomaticRecordingAPI).                         
- **Quick generation**: API Inspector can automatically generate API code snippets in various languages with prepopulated parameters, which can be run directly. For more information, please see [Quick API code generation](#AutomaticGeneratedAPI).                         
- **Online debugging**: API Inspector provides the quick online debugging feature of API Explorer, which can implement various features such as automatic multilingual SDK code generation, online call, real request sending, and automatic signature string generation, making the SDKs easier to use. For more information, please see [API Explorer online debugging](#APIExplorer).

## Features
### Enabling API Inspector
Please follow the steps below to enable the API Inspector feature:
1. Log in to the [CVM Console](https://console.cloud.tencent.com/cvm/instance/index?rid=1).      
2. Select <img src="https://main.qcloudimg.com/raw/4d7384619e988df262e740e376ed047c.png" style="margin:-4px 0px"> > **API** at the top of the page to enable API Inspector as shown below:
![](https://main.qcloudimg.com/raw/2895055159f741b2a05dce58b069ec94.png)      

<span id="AutomaticRecordingAPI"></span>
### Automatic API call recording
This document takes renaming an instance as an example to describe the automatic recording feature of API Inspector:
1. Rename an instance to "API Inspector". For detailed directions, please see [Renaming Instances](https://intl.cloud.tencent.com/document/product/213/16562).
2. Enable the API Inspector feature to view all API calls involved in the renaming operation as shown below:
![](https://main.qcloudimg.com/raw/d5bdeb299fe901b882b827f9dbb78b8b.png)
You can check "Hide Describe APIs" to view only the core feature-related APIs as shown below: 
![](https://main.qcloudimg.com/raw/ac224cdb58d6178ab55c093556816204.png)

<span id="AutomaticGeneratedAPI"></span>
### Quick API code generation
After an API involved in a console operation is recorded, you can click the API name to quickly generate API code snippets with prepopulated parameters in Java, Python, Node.js, PHP, Go, and .NET languages. You can select <img src="https://main.qcloudimg.com/raw/c87d07f7c2d1f7519b9b80f19d158e62.png" style="margin:-3px 0px"> to copy the code snippet in the corresponding format as shown below:
![](https://main.qcloudimg.com/raw/63773726c5bb91e99e8fd0123bd3e036.png)

<span id="APIExplorer"></span>
### API Explorer online debugging
You can select <img src="https://main.qcloudimg.com/raw/753d4cb89c7dc582b11ed66811143716.png" style="margin:-3px 0px"> or **Go to API Explorer** to debug the corresponding feature with API Explorer. You can also select <img src="https://main.qcloudimg.com/raw/68a84eebfed1c31bf37294902156d986.png" style="margin:-3px 0px"> to view the corresponding API documentation as shown below:
![](https://main.qcloudimg.com/raw/35beba6f9f0eaf41a463c353f2590191.png)
