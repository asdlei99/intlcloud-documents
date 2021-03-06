为方便 macOS 开发者调试和接入腾讯云游戏多媒体引擎产品 API，这里向您介绍适用于 macOS 开发的接入技术文档。

>?此文档对应 GME sdk version：2.7。

## 使用 GME 重要事项

|重要接口     | 接口含义|
| ------------- |:-------------:|
|InitEngine    				       	|初始化 GME 	|
|Poll    		|触发事件回调	|
|EnterRoom	 	|进房  		|
|EnableMic	 	|开麦克风 	|
|EnableSpeaker		|开扬声器 	|

>?
- GME 使用前请对工程进行配置，否则 SDK 不生效。
- GME 的接口调用成功后返回值为 QAVError.OK，数值为 0。
- GME 的接口调用要在同一个线程下。
- GME 需要周期性的调用 Poll 接口触发事件回调。
- GME 回调信息参考回调消息列表。
- 设备的操作要在进房成功之后。
- 错误码详情可参考 [错误码](https://intl.cloud.tencent.com/document/product/607/15173)。

## 实时语音流程图
![](https://main.qcloudimg.com/raw/e536525aa47c06a5a84bb6c8d4851b22.png)

## 核心接口
未初始化前，SDK 处于未初始化阶段，需要通过接口 Init 初始化 SDK，才可以使用实时语音及离线语音。
使用问题可参考 [一般性问题](https://intl.cloud.tencent.com/document/product/607/30254)。

|接口     | 接口含义   |
| ------------- |:-------------:|
|InitEngine    				       	|初始化 GME 	|
|Poll    	|触发事件回调	|
|Pause   	|系统暂停	|
|Resume 	|系统恢复	|
|Uninit    	|反初始化 GME 	|

### 获取单例
在使用语音功能时，需要首先获取 ITMGContext 对象。
####  函数原型 

```
ITMGContext ITMGDelegate <NSObject>
```



####  示例代码  

```
ITMGContext* _context = [ITMGContext GetInstance];
_context.TMGDelegate =self;
```

### 消息传递
接口类采用 Delegate 方法用于向应用程序发送回调通知，消息类型参考 ITMG_MAIN_EVENT_TYPE，消息内容为一个字典，用于接收回调的信息。
#### 函数原型

```
- (void)OnEvent:(ITMG_MAIN_EVENT_TYPE)eventType data:(NSDictionary*)data
```



####  示例代码  
```
-(void)OnEvent:(ITMG_MAIN_EVENT_TYPE)eventType data:(NSDictionary *)data{
    	NSLog(@"OnEvent:%lu,data:%@",(unsigned long)eventType,data);
		switch (eventType) {
			//对 eventType 进行判断
			}
	}
```





### 初始化 SDK

参数获取请查看 [接入指引](https://intl.cloud.tencent.com/document/product/607/10782)。
此接口需要来自腾讯云控制台的 AppID 号码作为参数，再加上 openID，这个 openID 是唯一标识一个用户，规则由 App 开发者自行制定，App 内不重复即可（目前只支持 INT64）。

>!初始化 SDK 之后才可以进房。

####  函数原型

```
ITMGContext -(int)InitEngine:(NSString*)sdkAppID openID:(NSString*)openId
```

|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| sdkAppId    	|NSString  |来自腾讯云控制台的 AppId 号码。				|
| openId    		|NSString  |OpenID 只支持 Int64 类型（转为 string 传入），数值必须大于10000，用于标识用户。 |


|返回值|处理|
|----|----|
|QAV_OK= 0|初始化 SDK 成功|
|QAV_ERR_SDK_NOT_FULL_UPDATE= 7015|检查 SDK 文件是否完整，建议删除后重新导入 SDK|

出现返回值 AV_ERR_SDK_NOT_FULL_UPDATE 时，此返回值只有提示作用，并不会造成初始化失败。
- 如果在接入过程中提示此错误，请根据提示检查 SDK 文件是否完整、SDK 文件版本是否一致。
- 如果是在导出可执行文件之后出现此返回值，请忽略此错误，并尽量不在 UI 中提示。

####  示例代码 


```
[[ITMGContext GetInstance] InitEngine:SDKAPPID3RD openID:_openId];
```
### 触发事件回调
通过在 update 里面周期的调用 Poll 可以触发事件回调。
####  函数原型

```
ITMGContext -(void)Poll
```
####  示例代码
```
[[ITMGContext GetInstance] Poll];
```

### 系统暂停
当系统发生 Pause 事件时，需要同时通知引擎进行 Pause。
####  函数原型

```
ITMGContext -(QAVResult)Pause
```

### 系统恢复
当系统发生 Resume 事件时，需要同时通知引擎进行 Resume。Resume 接口只恢复实时语音。
####  函数原型

```
ITMGContext -(QAVResult)Resume
```



### 反初始化 SDK
反初始化 SDK，进入未初始化状态。切换账号需要反初始化。
#### 函数原型

```
ITMGContext -(void)Uninit
```
####  示例代码
```
[[ITMGContext GetInstance] Uninit];
```


## 实时语音房间调用流程图

![](https://main.qcloudimg.com/raw/a61ca1d7cdecf09bd223766b2a5cd69f.png)

## 实时语音房间相关接口
初始化之后，SDK 调用进房后进去了房间，才可以进行实时语音通话。
使用问题可参考 [实时语音相关问题](https://intl.cloud.tencent.com/document/product/607/30257)。
 
|接口     | 接口含义   |
| ------------- |:-------------:|
|GenAuthBuffer    	|初始化鉴权|
|EnterRoom   		|加入房间|
|IsRoomEntered   	|是否已经进入房间|
|ExitRoom 		|退出房间|
|ChangeRoomType 	|修改用户房间音频类型|
|GetRoomType 		|获取用户房间音频类型|


### 鉴权信息
生成 AuthBuffer，用于相关功能的加密和鉴权，如正式发布请使用后台部署密钥，后台部署请参考 [鉴权密钥](https://intl.cloud.tencent.com/document/product/607/12218)。    
离线语音获取鉴权时，房间号参数必须填 null。

#### 函数原型
```
@interface QAVAuthBuffer : NSObject
+ (NSData*) GenAuthBuffer:(unsigned int)appId roomId:(NSString*)roomId openID:(NSString*)openID key:(NSString*)key;
+ @end
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| appId    		|int   		|来自腾讯云控制台的 AppId 号码。		|
| roomId    		|NSString  	|房间号，最大支持127字符（离线语音房间号参数必须填 null）。	|
| openID  		|NSString    	|用户标识。与 Init 时候的 openID相同。								|
| key    			|NSString    	|来自腾讯云 [控制台](https://console.cloud.tencent.com/gamegme) 的权限密钥。					|

####  示例代码  
```
NSData* authBuffer =   [QAVAuthBuffer GenAuthBuffer:SDKAPPID3RD.intValue roomId:_roomId openID:_openId key:AUTHKEY];
```



### 加入房间
用生成的鉴权信息进房，会收到消息为 ITMG_MAIN_EVENT_TYPE_ENTER_ROOM 的回调。加入房间默认不打开麦克风及扬声器。返回值为 AV_OK 的时候代表成功。


####  函数原型
```
ITMGContext   -(int)EnterRoom:(NSString*) roomId roomType:(int*)roomType authBuffer:(NSData*)authBuffer
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| roomId 	|NSString		|房间号，最大支持127字符|
| roomType 		|int			|房间音频类型		|
| authBuffer    	|NSData    	|鉴权码						|

房间音频类型请参考 [音质选择](https://intl.cloud.tencent.com/document/product/607/18522)。


####  示例代码  
```
[[ITMGContext GetInstance] EnterRoom:_roomId roomType:_roomType authBuffer:authBuffer];
```

### 加入房间事件的回调
加入房间完成后会发送信息 ITMG_MAIN_EVENT_TYPE_ENTER_ROOM，在 OnEvent 函数中进行判断。

```
- (void)OnEvent:(ITMG_MAIN_EVENT_TYPE)eventType data:(NSDictionary*)data
```
回调处理相关参考代码。

####  示例代码  
```
-(void)OnEvent:(ITMG_MAIN_EVENT_TYPE)eventType data:(NSDictionary *)data{
    NSLog(@"OnEvent:%lu,data:%@",(unsigned long)eventType,data);
    switch (eventType) {
        case ITMG_MAIN_EVENT_TYPE_ENTER_ROOM:
        {
            int result = ((NSNumber*)[data objectForKey:@"result"]).intValue;
            NSString* error_info = [data objectForKey:@"error_info"];
           	 //收到进房成功事件
        }
            break;
	}
}
```

#### Data详情
|消息     | Data         |例子|
| ------------- |:-------------:|------------- |
| ITMG_MAIN_EVENT_TYPE_ENTER_ROOM    				|result; error_info					|{"error_info":"","result":0}|

#### 错误码
|错误码值|原因及建议方案|
|-------|------------|
|7006|鉴权失败 有以下几个原因：1、AppID 不存在或者错误，2、authbuff 鉴权错误，3、鉴权过期 4、openID不符合规范。|
|7007|已经在其它房间。|
|1001   |已经在进房过程中，然后又重复了此操作。建议在进房回调返回之前不要再调用进房接口。|
|1003   |已经进房了在房间中，又调用一次进房接口。|
|1101   |确保已经初始化 SDK，或者确保在同一线程调用接口，以及确保 Poll 接口正常调用。|

### 判断是否已经进入房间
通过调用此接口可以判断是否已经进入房间，返回值为 bool 类型。
####  函数原型  
```
ITMGContext -(BOOL)IsRoomEntered
```
####  示例代码  
```
[[ITMGContext GetInstance] IsRoomEntered];
```

### 退出房间
通过调用此接口可以退出所在房间。这是一个异步接口，返回值为 AV_OK 的时候代表异步投递成功。

>!如果应用中有退房后立即进房的场景，在接口调用流程上，开发者无需要等待 ExitRoom 的回调 RoomExitComplete 通知，只需直接调用接口。

#### 函数原型  
```
ITMGContext -(int)ExitRoom
```
####  示例代码  
```
[[ITMGContext GetInstance] ExitRoom];
```

### 退出房间回调
退出房间完成后会有回调，消息为 ITMG_MAIN_EVENT_TYPE_EXIT_ROOM。
####  示例代码  
```
-(void)OnEvent:(ITMG_MAIN_EVENT_TYPE)eventType data:(NSDictionary *)data{
    NSLog(@"OnEvent:%lu,data:%@",(unsigned long)eventType,data);
    switch (eventType) {
        case ITMG_MAIN_EVENT_TYPE_EXIT_ROOM：
        {
            //收到退房成功事件
        }
            break;
    }
}
```

#### Data详情
|消息     | Data         |例子|
| ------------- |:-------------:|------------- |
| ITMG_MAIN_EVENT_TYPE_EXIT_ROOM    				|result; error_info  					|{"error_info":"","result":0}|


### 修改用户房间音频类型
此接口用于修改用户房间音频类型，结果参见回调事件，事件类型为 ITMG_MAIN_EVENT_TYPE_CHANGE_ROOM_TYPE。

####  函数原型  
```
ITMGContext GetRoom -(int)ChangeRoomType:(int)nRoomType
```


|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| nRoomType    |int    |希望房间切换成的类型，房间音频类型参考 EnterRoom 接口|

####  示例代码  
```
[[[ITMGContext GetInstance]GetRoom ]ChangeRoomType:_roomType];
```


### 获取用户房间音频类型
此接口用于获取用户房间音频类型，返回值为房间音频类型，返回值为0时代表获取用户房间音频类型发生错误，房间音频类型参考 EnterRoom 接口。

####  函数原型  
```
ITMGContext GetRoom -(int)GetRoomType
```

####  示例代码  
```
[[[ITMGContext GetInstance]GetRoom ]GetRoomType];
```


### 房间类型完成回调
房间类型设置完成后，回调的事件消息为 ITMG_MAIN_EVENT_TYPE_CHANGE_ROOM_TYPE，返回的参数为 result、error_info 及 new_room_type，new_room_type 代表的信息如下，在 OnEvent 函数中对事件消息进行判断。

|事件子类型     | 代表参数   |含义|
| ------------- |:-------------:|-------------|
| ITMG_ROOM_CHANGE_EVENT_ENTERROOM		|1 	|表示在进房的过程中，自带的音频类型与房间不符合，被修改为所进入房间的音频类型	|
| ITMG_ROOM_CHANGE_EVENT_START			|2	|表示已经在房间内，音频类型开始切换（例如调用 ChangeRoomType 接口后切换音频类型 ）|
| ITMG_ROOM_CHANGE_EVENT_COMPLETE		|3	|表示已经在房间，音频类型切换完成|
| ITMG_ROOM_CHANGE_EVENT_REQUEST			|4	|表示房间成员调用 ChangeRoomType 接口，请求切换房间音频类型|	


####  示例代码  
```
-(void)OnEvent:(ITMG_MAIN_EVENT_TYPE)eventType data:(NSDictionary *)data {
	NSLog(@"OnEvent:%lu,data:%@",(unsigned long)eventType,data);
    switch (eventType) {
 		case ITMG_MAIN_EVNET_TYPE_USER_UPDATE:
			//进行处理
	 }
    }
}
```

#### Data详情
|消息     | Data         |例子|
| ------------- |:-------------:|------------- |
| ITMG_MAIN_EVENT_TYPE_CHANGE_ROOM_TYPE    		|result; error_info; new_room_type	|{"error_info":"","new_room_type":0,"result":0}|


### 成员状态变化
该事件在状态变化才通知，状态不变化的情况下不通知。如需实时获取成员状态，请在上层收到通知时缓存，事件消息为 ITMG_MAIN_EVNET_TYPE_USER_UPDATE，其中 data 包含两个信息，event_id 及 user_list，在 OnEvent 函数中对事件消息进行判断。
音频事件的通知有一个阈值，超过这个阈值才会发送通知。超过两秒没有收到音频包才通知“有成员停止发送音频包”消息。

|event_id     | 含义         |应用侧维护内容|
| ------------- |:-------------:|-------------|
|ITMG_EVENT_ID_USER_ENTER    				|有成员进入房间			|应用侧维护成员列表		|
|ITMG_EVENT_ID_USER_EXIT    				|有成员退出房间			|应用侧维护成员列表		|
|ITMG_EVENT_ID_USER_HAS_AUDIO    		|有成员发送音频包		|应用侧维护通话成员列表	|
|ITMG_EVENT_ID_USER_NO_AUDIO    			|有成员停止发送音频包	|应用侧维护通话成员列表	|

#### 维护房间成员流程图

![](https://main.qcloudimg.com/raw/50ef1ceda14eb244e434b8cbe85da5f3.png)


#### 示例代码
```
-(void)OnEvent:(ITMG_MAIN_EVENT_TYPE)eventType data:(NSDictionary *)data{
    NSLog(@"OnEvent:%lu,data:%@",(unsigned long)eventType,data);
    switch (eventType) {
        case ITMG_MAIN_EVNET_TYPE_USER_UPDATE:
		{
		//进行处理
		//开发者对参数进行解析，得到信息 event_id及 user_list
		    switch (eventID)
 		    {
 		    case ITMG_EVENT_ID_USER_ENTER:
  			    //有成员进入房间
  			    break;
 		    case ITMG_EVENT_ID_USER_EXIT:
  			    //有成员退出房间
			    break;
		    case ITMG_EVENT_ID_USER_HAS_AUDIO:
			    //有成员发送音频包
			    break;
		    case ITMG_EVENT_ID_USER_NO_AUDIO:
			    //有成员停止发送音频包
			    break;
 		    }
		break;
		}
    }
}
```
### 房间通话质量监控事件
质量监控事件，在进房后触发，事件消息为 ITMG_MAIN_EVENT_TYPE_CHANGE_ROOM_QUALITY，返回的参数为 weight、loss 及 delay，代表的信息如下，在 OnEvent 函数中对事件消息进行判断。

|参数     | 类型        | 含义         |
| ------------- |-------------|-------------|
|weight  |int  				|范围是 1 - 50，数值为50是音质评分极好，数值为1是音质评分很差，几乎不能使用，数值为0代表初始值，无含义|
|loss   |double				|上行丢包率|
|delay   |int 		|音频触达延迟时间（ms）|




### 消息详情

|消息     | 消息代表的含义   
| ------------- |:-------------:|
|ITMG_MAIN_EVENT_TYPE_ENTER_ROOM    				       |进入音视频房间消息|
|ITMG_MAIN_EVENT_TYPE_EXIT_ROOM    				         	|退出音视频房间消息|
|ITMG_MAIN_EVENT_TYPE_ROOM_DISCONNECT    		       |房间因为网络等原因断开消息|
|ITMG_MAIN_EVENT_TYPE_CHANGE_ROOM_TYPE				|房间类型变化事件|

### 消息对应的Data详情
|消息     | Data         |例子|
| ------------- |:-------------:|------------- |
| ITMG_MAIN_EVENT_TYPE_ENTER_ROOM    				|result; error_info					|{"error_info":"","result":0}|
| ITMG_MAIN_EVENT_TYPE_EXIT_ROOM    				|result; error_info  					|{"error_info":"","result":0}|
| ITMG_MAIN_EVENT_TYPE_ROOM_DISCONNECT    		|result; error_info  					|{"error_info":"waiting timeout, please check your network","result":0}|
| ITMG_MAIN_EVENT_TYPE_CHANGE_ROOM_TYPE    		|result; error_info; new_room_type	|{"error_info":"","new_room_type":0,"result":0}|


## 实时语音音频接口
初始化 SDK 之后进房，在房间中，才可以调用实时音频语音相关接口。
当用户界面单击打开/关闭麦克风/扬声器按钮时，建议如下方式：
- 对于大部分的游戏类 App，推荐调用 EnableMic 及 EnableSpeaker 接口，相当于同时调用 EnableAudioCaptureDevice/EnableAudioSend 和 EnableAudioPlayDevice/EnableAudioRecv 接口；
- 其他类型的移动端 App 例如社交类型 App，打开或者关闭采集设备，会伴随整个设备（采集及播放）重启，如果此时 App 正在播放背景音乐，那么背景音乐的播放也会被中断。利用控制上下行的方式来实现开关麦克风效果，不会中断播放设备。具体调用方式为：在进房的时候调用 EnableAudioCaptureDevice(true) && EnableAudioPlayDevice(true) 一次，单击开关麦克风时只调用 EnableAudioSend/Recv 来控制音频流是否发送/接收。
- 如果想单独释放采集或者播放设备，请参考接口 EnableAudioCaptureDevice 及 EnableAudioPlayDevice。
- 调用 pause 暂停音频引擎，调用 resume 恢复音频引擎。

|接口     | 接口含义   |
| ------------- |:-------------:|
|EnableMic    						|开关麦克风|
|GetMicState    						|获取麦克风状态|
|EnableAudioCaptureDevice    		|开关采集设备		|
|IsAudioCaptureDeviceEnabled    	|获取采集设备状态	|
|EnableAudioSend    				|打开关闭音频上行	|
|IsAudioSendEnabled    				|获取音频上行状态	|
|GetMicLevel    						|获取实时麦克风音量	|
|GetSendStreamLevel					|获取音频上行实时音量|
|SetMicVolume    					|设置麦克风音量		|
|GetMicVolume    					|获取麦克风音量		|
|EnableSpeaker    					|开关扬声器|
|GetSpeakerState    					|获取扬声器状态|
|EnableAudioPlayDevice    			|开关播放设备		|
|IsAudioPlayDeviceEnabled    		|获取播放设备状态	|
|EnableAudioRecv    					|打开关闭音频下行	|
|IsAudioRecvEnabled    				|获取音频下行状态	|
|GetSpeakerLevel    					|获取实时扬声器音量	|
|GetRecvStreamLevel					|获取房间内其他成员下行实时音量|
|SetSpeakerVolume    				|设置扬声器音量		|
|GetSpeakerVolume    				|获取扬声器音量		|
|EnableLoopBack    					|开关耳返			|



### 开启关闭麦克风
此接口用来开启关闭麦克风。加入房间默认不打开麦克风及扬声器。
EnableMic = EnableAudioCaptureDevice + EnableAudioSend.
####  函数原型  
```
ITMGContext GetAudioCtrl -(QAVResult)EnableMic:(BOOL)enable
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| isEnabled    |boolean     |如果需要打开麦克风，则传入的参数为 YES，如果关闭麦克风，则参数为 NO|

####  示例代码  
```
打开麦克风
[[[ITMGContext GetInstance] GetAudioCtrl] EnableMic:YES];
```

### 麦克风状态获取
此接口用于获取麦克风状态，返回值0为关闭麦克风状态，返回值1 为打开麦克风状态。
####  函数原型  
```
ITMGContext GetAudioCtrl -(int)GetMicState
```
####  示例代码  
```
[[[ITMGContext GetInstance] GetAudioCtrl] GetMicState];
```

### 开启关闭采集设备
此接口用来开启/关闭采集设备。加入房间默认不打开设备。
- 只能在进房后调用此接口，退房会自动关闭设备。
- 在移动端，打开采集设备通常会伴随权限申请，音量类型调整等操作。

####  函数原型  
```
ITMGContext GetAudioCtrl -(QAVResult)EnableAudioCaptureDevice:(BOOL)enabled
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| enabled    |BOOL     |如果需要打开采集设备，则传入的参数为 YES，如果关闭采集设备，则参数为 NO|

#### 示例代码

```
打开采集设备
[[[ITMGContext GetInstance]GetAudioCtrl ]EnableAudioCaptureDevice:enabled];
```

### 采集设备状态获取
此接口用于采集设备状态获取。
#### 函数原型

```
ITMGContext GetAudioCtrl -(BOOL)IsAudioCaptureDeviceEnabled
```
#### 示例代码

```
BOOL IsAudioCaptureDevice = [[[ITMGContext GetInstance] GetAudioCtrl] IsAudioCaptureDeviceEnabled];
```

### 打开关闭音频上行
此接口用于打开/关闭音频上行。如果采集设备已经打开，那么会发送采集到的音频数据。如果采集设备没有打开，那么仍旧无声。采集设备的打开关闭参见接口 EnableAudioCaptureDevice。

#### 函数原型

```
ITMGContext GetAudioCtrl -(QAVResult)EnableAudioSend:(BOOL)enable
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| enable    |BOOL     |如果需要打开音频上行，则传入的参数为 YES，如果关闭音频上行，则参数为 NO|

#### 示例代码  

```
[[[ITMGContext GetInstance]GetAudioCtrl ]EnableAudioSend:enabled];
```

### 音频上行状态获取
此接口用于音频上行状态获取。
#### 函数原型  
```
ITMGContext GetAudioCtrl -(BOOL)IsAudioSendEnabled
```
####  示例代码  
```
BOOL IsAudioSend =  [[[ITMGContext GetInstance] GetAudioCtrl] IsAudioSendEnabled];
```

### 获取麦克风实时音量
此接口用于获取麦克风实时音量，返回值为 int 类型。建议 20ms 获取一次。
####  函数原型  
```
ITMGContext GetAudioCtrl -(int)GetMicLevel
```
####  示例代码  
```
[[[ITMGContext GetInstance] GetAudioCtrl] GetMicLevel];
```

### 获取音频上行实时音量
此接口用于获取音频上行实时音量，返回值为 int 类型，取值范围为0到100。
####  函数原型  
```
ITMGContext GetAudioCtrl -(int)GetSendStreamLevel()
```
####  示例代码  
```
[[[ITMGContext GetInstance] GetAudioCtrl] GetSendStreamLevel];
```

### 设置麦克风的音量
此接口用于设置麦克风的音量。参数 volume 用于设置麦克风的音量，当数值为0的时候表示静音，当数值为100 的时候表示音量不增不减，默认数值为100。

####  函数原型  
```
ITMGContext GetAudioCtrl -(QAVResult)SetMicVolume:(int) volume
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| volume    |int      |设置音量，范围0到200|

#### 示例代码  
```
[[[ITMGContext GetInstance] GetAudioCtrl] SetMicVolume:100];
```
###  获取麦克风的音量
此接口用于获取麦克风的音量。返回值为一个int类型数值，返回值为101代表没调用过接口 SetMicVolume。

#### 函数原型  
```
ITMGContext GetAudioCtrl -(int) GetMicVolume
```
####  示例代码  
```
[[[ITMGContext GetInstance] GetAudioCtrl] GetMicVolume];
```

### 开启关闭扬声器
此接口用于开启关闭扬声器。
EnableSpeaker = EnableAudioPlayDevice +  EnableAudioRecv.
####  函数原型  
```
ITMGContext GetAudioCtrl -(void)EnableSpeaker:(BOOL)enable
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| isEnabled    |boolean       |如果需要关闭扬声器，则传入的参数为 NO，如果打开扬声器，则参数为 YES|

####  示例代码  
```
[[[ITMGContext GetInstance] GetAudioCtrl] EnableSpeaker:YES];
```

### 扬声器状态获取
此接口用于扬声器状态获取。返回值 0 为关闭扬声器状态，返回值1 为打开扬声器状态，返回值2 为扬声器设备正在操作中。
####  函数原型  
```
ITMGContext GetAudioCtrl -(int)GetSpeakerState
```

####  示例代码  
```
[[[ITMGContext GetInstance] GetAudioCtrl] GetSpeakerState];
```



### 开启关闭播放设备
此接口用于开启关闭播放设备。

#### 函数原型  
```
ITMGContext GetAudioCtrl -(QAVResult)EnableAudioPlayDevice:(BOOL)enabled
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| enabled    |BOOL        |如果需要关闭播放设备，则传入的参数为 NO，如果打开播放设备，则参数为 YES|

#### 示例代码  
```
打开播放设备
[[[ITMGContext GetInstance]GetAudioCtrl ]EnableAudioPlayDevice:enabled];
```

### 播放设备状态获取
此接口用于播放设备状态获取。
#### 函数原型

```
ITMGContext GetAudioCtrl -(BOOL)IsAudioPlayDeviceEnabled
```
#### 示例代码  

```
BOOL IsAudioPlayDevice =  [[[ITMGContext GetInstance] GetAudioCtrl] IsAudioPlayDeviceEnabled];
```

### 打开关闭音频下行
此接口用于打开/关闭音频下行。如果播放设备已经打开，那么会播放房间里其他人的音频数据。如果播放设备没有打开，那么仍旧无声。播放设备的打开关闭参见接口 参见 EnableAudioPlayDevice。

#### 函数原型  

```
ITMGContext GetAudioCtrl -(QAVResult)EnableAudioRecv:(BOOL)enabled
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| enabled    |BOOL     |如果需要打开音频下行，则传入的参数为 YES，如果关闭音频下行，则参数为 NO|

#### 示例代码  

```
[[[ITMGContext GetInstance]GetAudioCtrl ]EnableAudioRecv:enabled];
```



### 音频下行状态获取
此接口用于音频下行状态获取。
#### 函数原型  
```
ITMGAudioCtrl bool IsAudioRecvEnabled()
```

####  示例代码  
```
BOOL IsAudioRecv = [[[ITMGContext GetInstance] GetAudioCtrl] IsAudioRecvEnabled];
```

### 获取扬声器实时音量
此接口用于获取扬声器实时音量。返回值为 int 类型数值，表示扬声器实时音量。建议 20ms 获取一次。
####  函数原型  
```
ITMGContext GetAudioCtrl -(int)GetSpeakerLevel
```

####  示例代码  
```
[[[ITMGContext GetInstance] GetAudioCtrl] GetSpeakerLevel];
```

### 获取房间内其他成员下行实时音量
此接口用于获取房间内其他成员下行实时音量，返回值为 int 类型，取值范围为0到100。
####  函数原型  
```
ITMGContext GetAudioCtrl -(int)GetRecvStreamLevel:(NSString*) openID 
```

|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| openID    |NSString       |房间其他成员的 openId|

####  示例代码  
```
[[[ITMGContext GetInstance] GetAudioCtrl] GetRecvStreamLevel:(NSString*) openId
```

### 设置扬声器的音量
此接口用于设置扬声器的音量。
参数 volume 用于设置扬声器的音量，当数值为0时，表示静音，当数值为100时，表示音量不增不减，默认数值为 100。

####  函数原型  
```
ITMGContext GetAudioCtrl -(QAVResult)SetSpeakerVolume:(int)vol
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| vol    |int        |设置音量，范围 0 到 200|

#### 示例代码  
```
[[[ITMGContext GetInstance] GetAudioCtrl] SetSpeakerVolume:100];
```

### 获取扬声器的音量

此接口用于获取扬声器的音量。返回值为 int 类型数值，代表扬声器的音量，返回值为101代表没调用过接口 SetSpeakerVolume。
Level 是实时音量，Volume 是扬声器的音量，最终声音音量相当于 Level*Volume%。举个例子：实时音量是数值是 100 的话，此时Volume的数值是 60，那么最终发出来的声音数值也是 60。

####  函数原型  
```
ITMGContext GetAudioCtrl -(int)GetSpeakerVolume
```
####  示例代码  
```
[[[ITMGContext GetInstance] GetAudioCtrl] GetSpeakerVolume];
```


### 启动耳返
此接口用于启动耳返，需要 EnableLoopBack+EnableSpeaker 才可以听到自己声音。

####  函数原型  
```
ITMGContext GetAudioCtrl -(QAVResult)EnableLoopBack:(BOOL)enable
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| enable    |boolean         |设置是否启动|

####  示例代码  
```
[[[ITMGContext GetInstance] GetAudioCtrl] EnableLoopBack:YES];
```


## 离线语音语音转文字流程图
<img src="https://main.qcloudimg.com/raw/4c875d05cd2b4eaefba676d2e4fc031d.png" width="70%">


## 离线语音
未初始化前，SDK 处于未初始化阶段，需要通过接口 Init 初始化 SDK，才可以使用实时语音及离线语音。
使用问题可参考 [离线语音相关问题](https://intl.cloud.tencent.com/document/product/607/30258)。


### 初始化相关接口

|接口     | 接口含义   |
| ------------- |:-------------:|
|Init    	|初始化 GME	| 
|Poll    	|触发事件回调	|
|Pause   	|系统暂停	|
|Resume 	|系统恢复	|
|Uninit    	|反初始化 GME 	|


### 离线语音相关接口
|接口     | 接口含义   |
| ------------- |:-------------:|
|ApplyPTTAuthbuffer    |鉴权初始化	|
|SetMaxMessageLength    |限制最大语音信息时长	|
|StartRecording		|启动录音		|
|StartRecordingWithStreamingRecognition		|启动流式录音		|
|PauseRecording|暂停录音|
|ResumeRecording|恢复录音|
|StopRecording    	|停止录音		|
|CancelRecording	|取消录音		|
|GetMicLevel|获取离线语音实时麦克风音量|
|SetMicVolume|设置离线语音录制音量|
|GetMicVolume|获取离线语音录制音量|
|GetSpeakerLevel|获取离线语音实时扬声器音量  |
|SetSpeakerVolume|设置离线语音播放音量|
|GetSpeakerVolume|获取离线语音播放音量|
|UploadRecordedFile 	|上传语音文件		|
|DownloadRecordedFile	|下载语音文件		|
|PlayRecordedFile 	|播放语音		|
|StopPlayFile		|停止播放语音		|
|GetFileSize 		|语音文件的大小		|
|GetVoiceFileDuration	|语音文件的时长		|
|SpeechToText 		|语音识别成文字		|

### 鉴权初始化
在初始化 SDK 之后调用鉴权初始化，authBuffer 的获取参见上文实时语音鉴权信息接口。
#### 函数原型  
```
ITMGContext GetPTT -(QAVResult)ApplyPTTAuthbuffer:(NSData *)authBuffer
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| authBuffer    |NSData*                    |鉴权|

#### 示例代码  
```
[[[ITMGContext GetInstance]GetPTT]ApplyPTTAuthbuffer:(NSData *)authBuffer];
```

### 限制最大语音信息时长
限制最大语音消息的长度，最大支持58秒。

#### 函数原型

```
ITMGContext GetPTT -(QAVResult)SetMaxMessageLength:(int)msTime
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| msTime    |int                    |语音时长，单位 ms，区间为 1000 < msTime <= 58000|

#### 示例代码  
```
[[[ITMGContext GetInstance]GetPTT]SetMaxMessageLength:(int)msTime];
```


### 启动录音
此接口用于启动录音。需要将录音文件上传后才可以进行语音转文字等操作。
####  函数原型  
```
ITMGContext GetPTT -(int)StartRecording:(NSString*)fileDir
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| fileDir    |NSString                     |存放的语音路径|

####  示例代码  
```
[[[ITMGContext GetInstance]GetPTT]StartRecording:path];
```

### 启动录音的回调
启动录音完成后的回调调用函数 OnEvent，事件消息为 ITMG_MAIN_EVNET_TYPE_PTT_RECORD_COMPLETE， 在 OnEvent 函数中对事件消息进行判断。
传递的参数包含两个信息，一个是 result，另一个是 file_path。

#### 错误码
|错误码值 |原因  |建议方案        |
|-----|------|------|
|4097   |参数为空|检查代码中接口参数是否正确|
|4098   |初始化错误|检查设备是否被占用，或者权限是否正常，是否初始化正常|
|4099   |正在录制中|确保在正确的时机使用 SDK 录制功能|
|4100   |没有采集到音频数据|检查麦克风设备是否正常|
|4101   |录音时，录制文件访问错误|确保文件存在，文件路径的合法性|
|4102   |麦克风未授权错误|使用 SDK 需要麦克风权限，添加权限请参考对应引擎或平台的 SDK 工程配置文档|
|4103   |录音时间太短错误|首先，限制录音时长的单位为毫秒，检查参数是否正确；其次，录音时长要1000毫秒以上才能成功录制|
|4104   |没有启动录音操作|检查是否已经调用启动录音接口|

####  示例代码  
```
-(void)OnEvent:(ITMG_MAIN_EVENT_TYPE)eventType data:(NSDictionary *)data{
    NSLog(@"OnEvent:%lu,data:%@",(unsigned long)eventType,data);
    switch (eventType) {
        case ITMG_MAIN_EVNET_TYPE_PTT_RECORD_COMPLETE：
        {
	    //录音回调
        }
            break;
    }
}
```

### 启动流式语音识别
此接口用于启动流式语音识别，同时在回调中会有实时的语音转文字返回，可以指定语言进行识别，也可以将语音中识别到的信息翻译成指定的语言返回。

#### 函数原型  

```
ITMGContext GetPTT -(void) StartRecordingWithStreamingRecognition(const NSString* filePath)
ITMGContext GetPTT -(void) StartRecordingWithStreamingRecognition(const NSString* filePath,const NSString*speechLanguage,const NSString*translateLanguage)
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| filePath    	|NSString* 	|存放的语音路径	|
| speechLanguage    |NSString*                     |识别成指定文字的语言参数，参数请参考 [语音转文字的语言参数参考列表](https://intl.cloud.tencent.com/document/product/607/30260)|
| translateLanguage    |NSString*                     |翻译成指定文字的语言参数，参数请参考 [语音转文字的语言参数参考列表](https://intl.cloud.tencent.com/document/product/607/30260)（此参数暂不可用,请填写与 speechLanguage 相同的参数）|

#### 示例代码  
```
[[[ITMGContext GetInstance] GetPTT] StartRecordingWithStreamingRecognition:recordfilePath  speechLanguage:@"cmn-Hans-CN" translateLanguage:@"cmn-Hans-CN"];
```

### 启动流式语音识别的回调
启动流式语音识别完成后的回调调用函数 OnEvent，事件消息为 ITMG_MAIN_EVNET_TYPE_PTT_STREAMINGRECOGNITION_COMPLETE， 在 OnEvent 函数中对事件消息进行判断。传递的参数包含以下四个信息。

|消息名称     | 含义         |
| ------------- |:-------------:|
| result    	|用于判断流式语音识别是否成功的返回码		|
| text    		|语音转文字识别的文本	|
| file_path 	|录音存放的本地地址		|
| file_id 		|录音在后台的 url 地址，录音在服务器存放90天	|

|错误码     | 含义         |处理方式|
| ------------- |:-------------:|:-------------:|
|32775	|流式语音转文本失败，但是录音成功	|调用 UploadRecordedFile 接口上传录音，再调用 SpeechToText 接口进行语音转文字操作
|32777	|流式语音转文本失败，但是录音成功，上传成功	|返回的信息中有上传成功的后台 url 地址，调用 SpeechToText 接口进行语音转文字操作

#### 示例代码  
```
-(void)OnEvent:(ITMG_MAIN_EVENT_TYPE)eventType data:(NSDictionary *)data{
    NSLog(@"OnEvent:%lu,data:%@",(unsigned long)eventType,data);
    switch (eventType) {
        case ITMG_MAIN_EVNET_TYPE_PTT_STREAMINGRECOGNITION_COMPLETE：
        {
	    //流式语音识别的回调
        }
            break;
    }
}
```
### 暂停录音
此接口用于暂停录音。如需恢复录音请调用接口 ResumeRecording。

#### 函数原型  
```
ITMGContext GetPTT int PauseRecording()
```
####  示例代码  
```
[[[ITMGContext GetInstance]GetPTT]PauseRecording;
```

### 恢复录音
此接口用于恢复录音。

#### 函数原型  
```
ITMGContext GetPTT int ResumeRecording()
```
####  示例代码  
```
[[[ITMGContext GetInstance]GetPTT]ResumeRecording;
```


### 停止录音
此接口用于停止录音。此接口为异步接口，停止录音后会有录音完成回调，成功之后录音文件才可用。
#### 函数原型  
```
ITMGContext GetPTT -(QAVResult)StopRecording
```
####  示例代码  
```
[[[ITMGContext GetInstance]GetPTT]StopRecording];
```



### 取消录音
调用此接口取消录音。取消之后没有回调。
#### 函数原型  
```
ITMGContext GetPTT -(QAVResult)CancelRecording
```
####  示例代码  
```
[[[ITMGContext GetInstance]GetPTT]CancelRecording];
```

### 获取离线语音麦克风实时音量
此接口用于获取麦克风实时音量，返回值为 int 类型，值域为0到100。

#### 函数原型  
```
ITMGContext GetPTT -(QAVResult)GetMicLevel
```
#### 示例代码  
```
[[[ITMGContext GetInstance]GetPTT]GetMicLevel];
```

### 设置离线语音录制音量
此接口用于设置离线语音录制音量，值域为0到100。

#### 函数原型  
```
ITMGContext GetPTT int SetMicVolume:(int) vol
```
#### 示例代码  
```
[[[ITMGContext GetInstance]GetPTT]SetMicVolume:100];
```

### 获取离线语音录制音量
此接口用于获取离线语音录制音量。返回值为 int 类型，值域为0到100。

#### 函数原型  
```
ITMGContext GetPTT int GetMicVolume()
```

#### 示例代码  
```
[[[ITMGContext GetInstance]GetPTT]GetMicVolume];
```


### 获取扬声器实时音量
此接口用于获取扬声器实时音量。返回值为 int 类型，值域为0到100。

#### 函数原型  
```
ITMGContext GetPTT -(QAVResult)GetSpeakerLevel
```

#### 示例代码  
```
[[[ITMGContext GetInstance]GetPTT]GetSpeakerLevel];
```

### 设置离线语音播放音量
此接口用于设置离线语音播放音量，值域为0到100。

#### 函数原型  
```
ITMGContext GetPTT int SetSpeakerVolume:(int) vol
```
#### 示例代码  
```
[[[ITMGContext GetInstance]GetPTT]SetSpeakerVolume:100];
```

### 获取离线语音播放音量
此接口用于获取离线语音播放音量。返回值为 int 类型，值域为0到100。

#### 函数原型  
```
ITMGContext GetPTT int GetSpeakerVolume()
```

#### 示例代码  
```
[[[ITMGContext GetInstance]GetPTT]GetSpeakerVolume];
```




### 上传语音文件
此接口用于上传语音文件。
#### 函数原型  
```
ITMGContext GetPTT -(void)UploadRecordedFile:(NSString*)filePath 
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| filePath    |NSString                      |上传的语音路径|

#### 示例代码  
```
[[[ITMGContext GetInstance]GetPTT]UploadRecordedFile:path];
```


### 上传语音完成的回调
上传语音完成后，事件消息为 ITMG_MAIN_EVNET_TYPE_PTT_UPLOAD_COMPLETE， 在 OnEvent 函数中对事件消息进行判断。
#### 错误码

|错误码值 |原因  |建议方案        |
|-----|------|------|
|8193   |上传文件时，文件访问错误|确保文件存在，文件路径的合法性|
|8194   |签名校验失败错误|检查鉴权密钥是否正确，检查是否有初始化离线语音|
|8195   |网络错误|检查设备网络是否可以正常访问外网环境|
|8196   |获取上传参数过程中网络失败|检查鉴权是否正确，检查设备网络是否可以正常访问外网环境|
|8197   |获取上传参数过程中回包数据为空|检查鉴权是否正确，检查设备网络是否可以正常访问外网环境|
|8198   |获取上传参数过程中回包解包失败|检查鉴权是否正确，检查设备网络是否可以正常访问外网环境|
|8200   |没有设置 appinfo|检查 apply 接口是否有调用，或者入参是否为空|

#### 示例代码

```
-(void)OnEvent:(ITMG_MAIN_EVENT_TYPE)eventType data:(NSDictionary *)data{
    NSLog(@"OnEvent:%lu,data:%@",(unsigned long)eventType,data);
    switch (eventType) {
        case ITMG_MAIN_EVNET_TYPE_PTT_UPLOAD_COMPLETE：
        {
	    //上传语音成功
        }
            break;
    }
}
```


### 下载语音文件
此接口用于下载语音文件。
#### 函数原型  
```
ITMGContext GetPTT -(void)DownloadRecordedFile:(NSString*)fileId downloadFilePath:(NSString*)downloadFilePath 
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| fileID    			|NSString                      |文件的url路径，录音在服务器存放90天		|
| downloadFilePath 	|NSString                      |文件的本地保存路径	|

####  示例代码  
```
[[[ITMGContext GetInstance]GetPTT]DownloadRecordedFile:fileIdpath downloadFilePath:path];
```


### 下载语音文件完成回调
下载语音完成后，事件消息为 ITMG_MAIN_EVNET_TYPE_PTT_DOWNLOAD_COMPLETE， 在 OnEvent 函数中对事件消息进行判断。
传递的参数包含三个信息，result、file_path 和 file_id。
#### 错误码

|错误码值 |原因  |建议方案        |
|-----|------|------|
|12289  |下载文件时，文件访问错误    |检查文件路径是否合法|
|12290  |签名校验失败    |检查鉴权密钥是否正确，检查是否有初始化离线语音|
|12291  |网络存储系统异常    |服务器获取语音文件失败，检查接口参数 fileid 是否正确，检查网络是否正常，检查 cos 文件存不存在|
|12292  |服务器文件系统错误    |检查设备网络是否可以正常访问外网环境，检查服务器上是否有此文件|
|12293  |获取下载参数过程中，HTTP 网络失败|检查设备网络是否可以正常访问外网环境|
|12294  |获取下载参数过程中，回包数据为空 |检查设备网络是否可以正常访问外网环境|
|12295  |获取下载参数过程中，回包解包失败|检查设备网络是否可以正常访问外网环境|
|12297  |没有设置 appinfo|检查鉴权密钥是否正确，检查是否有初始化离线语音|


#### 示例代码

```
-(void)OnEvent:(ITMG_MAIN_EVENT_TYPE)eventType data:(NSDictionary *)data{
    NSLog(@"OnEvent:%lu,data:%@",(unsigned long)eventType,data);
    switch (eventType) {
        case ITMG_MAIN_EVNET_TYPE_PTT_DOWNLOAD_COMPLETE：
        {
	    //下载成功   
        }
            break;
    }
}
```



### 播放语音
此接口用于播放语音。
####  函数原型  
```
ITMGContext GetPTT -(void)PlayRecordedFile:(NSString*)downloadFilePath
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| downloadFilePath    |NSString                      |文件的路径|

#### 错误码

|错误码值 |原因  |建议方案        |
|-----|------|------|
|20485  |播放未开始|确保文件存在，文件路径的合法性|

####  示例代码  
```
[[[ITMGContext GetInstance]GetPTT]PlayRecordedFile:path];
```


### 播放语音的回调
播放语音的回调，事件消息为 ITMG_MAIN_EVNET_TYPE_PTT_PLAY_COMPLETE， 在 OnEvent 函数中对事件消息进行判断。
传递的参数包含两个信息，一个是 result，另一个是 file_path。

#### 错误码

|错误码值 |原因  |建议方案        |
|-----|------|------|
|20481  |初始化错误|检查设备是否被占用，或者权限是否正常，是否初始化正常|
|20482  |正在播放中，试图打断并播放下一个失败了（正常是可以打断的）|检查代码逻辑是否正确|
|20483  |参数为空|检查代码中接口参数是否正确|
|20484  |内部错误|初始化播放器错误，解码失败等问题产生此错误码，需要结合日志定位问题|

#### 示例代码

```
-(void)OnEvent:(ITMG_MAIN_EVENT_TYPE)eventType data:(NSDictionary *)data{
    NSLog(@"OnEvent:%lu,data:%@",(unsigned long)eventType,data);
    switch (eventType) {
        case ITMG_MAIN_EVNET_TYPE_PTT_PLAY_COMPLETE：
        {
	    //播放语音的回调 
        }
            break;
    }
}
```




### 停止播放语音
此接口用于停止播放语音。停止播放语音也会有播放完成的回调。
####  函数原型  
```
ITMGContext GetPTT -(int)StopPlayFile
```

####  示例代码  
```
[[[ITMGContext GetInstance]GetPTT]StopPlayFile];
```



### 获取语音文件的大小
通过此接口，获取语音文件的大小。
####  函数原型  
```
ITMGContext GetPTT -(int)GetFileSize:(NSString*)filePath
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| filePath    |NSString                     |语音文件的路径|

####  示例代码  
```
[[[ITMGContext GetInstance]GetPTT]GetFileSize:path];
```

### 获取语音文件的时长
此接口用于获取语音文件的时长，单位毫秒。
####  函数原型  
```
ITMGContext GetPTT -(int)GetVoiceFileDuration:(NSString*)filePath
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| filePath    |NSString                     |语音文件的路径|

####  示例代码  
```
[[[ITMGContext GetInstance]GetPTT]GetVoiceFileDuration:path];
```


### 将指定的语音文件识别成文字
此接口用于将指定的语音文件识别成文字。

#### 函数原型  
```
ITMGContext GetPTT -(void)SpeechToText:(NSString*)fileID
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| fileID    |NSString                     |语音文件 url，录音在服务器存放90天|

#### 示例代码  
```
[[[ITMGContext GetInstance]GetPTT]SpeechToText:fileID];
```



### 将指定的语音文件翻译成文字（指定语言）
此接口可以指定语言进行识别，也可以将语音中识别到的信息翻译成指定的语言返回。

#### 函数原型  
```
ITMGContext GetPTT -(void)SpeechToText:(NSString*)fileID (NSString*)speechLanguage (NSString*)translateLanguage
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| fileID    |NSString*                     |语音文件 url，录音在服务器存放 90 天|
| speechLanguage    |NSString*                     |识别出指定文字的语言参数，参数参考[语音转文字的语言参数参考列表](https://intl.cloud.tencent.com/document/product/607/30260)|
| translatelanguage    |NSString*                     |翻译成指定文字的语言参数，参数参考[语音转文字的语言参数参考列表](https://intl.cloud.tencent.com/document/product/607/30260)（此参数暂时无效，填入参数应与 speechLanguage 一致）|

#### 示例代码  
```
[[[ITMGContext GetInstance]GetPTT]SpeechToText:fileID speechLanguage:"cmn-Hans-CN" translateLanguage:"cmn-Hans-CN"];
```



### 识别回调
将指定的语音文件识别成文字的回调，事件消息为 ITMG_MAIN_EVNET_TYPE_PTT_SPEECH2TEXT_COMPLETE， 在 OnEvent 函数中对事件消息进行判断。
传递的参数包含三个信息，result、file_path 和 text，其中 text 为识别的文本。

#### 错误码

|错误码值 |原因  |建议方案        |
|-----|------|------|
|32769  |内部错误|分析日志，获取后台返回给客户端的真正错误码，并联系后台同事协助解决。|
|32770  |网络失败|检查设备网络是否可以正常访问外网环境|
|32772  |回包解包失败|分析日志，获取后台返回给客户端的真正错误码，并联系后台同事协助解决。|
|32774  |没有设置 appinfo|检查鉴权密钥是否正确，检查是否有初始化离线语音|
|32776  |authbuffer 校验失败|检查 authbuffer 是否正确|
|32784  |语音转文本参数错误|检查代码中接口参数 fileid 是否为空|
|32785  |语音转文本翻译返回错误|离线语音后台错误，请分析日志，获取后台返回给客户端的真正错误码，并联系后台同事协助解决|

#### 示例代码

```
-(void)OnEvent:(ITMG_MAIN_EVENT_TYPE)eventType data:(NSDictionary *)data{
    NSLog(@"OnEvent:%lu,data:%@",(unsigned long)eventType,data);
    switch (eventType) {
        case ITMG_MAIN_EVNET_TYPE_PTT_SPEECH2TEXT_COMPLETE：
        {
	    //成功识别语音文件       
        }
            break;   
    }
}
```



## 高级 API

### 获取版本号
获取 SDK 版本号，用于分析。
#### 函数原型
```
ITMGContext  -(NSString*)GetSDKVersion
```
#### 示例代码  
```
[[ITMGContext GetInstance] GetSDKVersion];
```

### 设置打印日志等级
用于设置打印日志等级。建议保持默认等级。
#### 函数原型
```
ITMGContext -(void)SetLogLevel:(ITMG_LOG_LEVEL)levelWrite (ITMG_LOG_LEVEL)levelPrint
```

#### 参数含义

|参数|类型|含义|
|---|---|---|
|levelWrite|ITMG_LOG_LEVEL|设置写入日志的等级，TMG_LOG_LEVEL_NONE 表示不写入|
|levelPrint|ITMG_LOG_LEVEL|设置打印日志的等级，TMG_LOG_LEVEL_NONE 表示不打印|



|ITMG_LOG_LEVEL|含义|
| -------------------------------|----------------------|
|TMG_LOG_LEVEL_NONE=0		|不打印日志			|
|TMG_LOG_LEVEL_ERROR=1		|打印错误日志（默认）	|
|TMG_LOG_LEVEL_INFO=2			|打印提示日志		|
|TMG_LOG_LEVEL_DEBUG=3		|打印开发调试日志	|
|TMG_LOG_LEVEL_VERBOSE=4		|打印高频日志		|

#### 示例代码  
```
[[ITMGContext GetInstance] SetLogLevel:TMG_LOG_LEVEL_INFO TMG_LOG_LEVEL_INFO];
```



### 设置打印日志路径
用于设置打印日志路径。默认路径为： /Users/username/Library/Containers/xxx.xxx.xxx/Data/Documents。
#### 函数原型
```
ITMGContext -(void)SetLogPath:(NSString*)logDir
```

|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| logDir    		|NSString   		|路径|

#### 示例代码  
```
[[ITMGContext GetInstance] SetLogPath:Path];
```


### 获取诊断信息
获取音视频通话的实时通话质量的相关信息。该接口主要用来查看实时通话质量、排查问题等，业务侧可以忽略。
#### 函数原型  
```
ITMGContext GetRoom -(NSString*)GetQualityTips
```
#### 示例代码  
```
[[[ITMGContext GetInstance]GetRoom ] GetQualityTips];
```

### 加入音频数据黑名单
将某个 ID 加入音频数据黑名单，即不接受某人的语音， 只对本端生效。返回值为 0 表示调用成功。例如 ：A，B，C 在同一个房间开麦说话：
- 如果 A 设置了 C 的黑名单， 则 A 只能听见 B 的声音。
- B 因为没有设置黑名单， 仍旧可以听见 A 和 C 的声音。
- C 同样因为没有设置黑名单， 可以听见 A 和 B 的声音。

#### 函数原型  

```
ITMGContext GetAudioCtrl -(QAVResult)AddAudioBlackList:(NSString*)openID
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| openId    |NSString      |需添加黑名单的 ID|

#### 示例代码  

```
[[[ITMGContext GetInstance]GetAudioCtrl ] AddAudioBlackList[id]];
```

### 移除音频数据黑名单
将某个 ID 移除音频数据黑名单。返回值为 0 表示调用成功。
#### 函数原型  

```
ITMGContext GetAudioCtrl -(QAVResult)RemoveAudioBlackList:(NSString*)openID
```
|参数     | 类型         |含义|
| ------------- |:-------------:|-------------|
| openId    |NSString      |需移除黑名单的 ID|

#### 示例代码  

```
[[[ITMGContext GetInstance]GetAudioCtrl ] RemoveAudioBlackList[openId]];
```


## 回调消息

### 消息列表

|消息     | 消息代表的含义   
| ------------- |:-------------:|
|ITMG_MAIN_EVENT_TYPE_ENTER_ROOM    		|进入音频房间消息		|
|ITMG_MAIN_EVENT_TYPE_EXIT_ROOM    		|退出音频房间消息		|
|ITMG_MAIN_EVENT_TYPE_ROOM_DISCONNECT		|房间因为网络等原因断开消息	|
|ITMG_MAIN_EVENT_TYPE_CHANGE_ROOM_TYPE		|房间类型变化事件		|
|ITMG_MAIN_EVENT_TYPE_ACCOMPANY_FINISH		|伴奏结束消息			|
|ITMG_MAIN_EVNET_TYPE_USER_UPDATE		|房间成员更新消息		|
|ITMG_MAIN_EVENT_TYPE_NUMBER_OF_USERS_UPDATE|房间成员数量更新消息		|
|ITMG_MAIN_EVENT_TYPE_NUMBER_OF_AUDIOSTREAMS_UPDATE|房间音频流数量更新消息		|
|ITMG_MAIN_EVENT_TYPE_CHANGE_ROOM_QUALITY		|房间质量信息		|
|ITMG_MAIN_EVNET_TYPE_PTT_RECORD_COMPLETE	|PTT 录音完成			|
|ITMG_MAIN_EVNET_TYPE_PTT_UPLOAD_COMPLETE	|上传 PTT 完成			|
|ITMG_MAIN_EVNET_TYPE_PTT_DOWNLOAD_COMPLETE	|下载 PTT 完成			|
|ITMG_MAIN_EVNET_TYPE_PTT_PLAY_COMPLETE		|播放 PTT 完成			|
|ITMG_MAIN_EVNET_TYPE_PTT_SPEECH2TEXT_COMPLETE	|语音转文字完成			|

### Data 列表

|消息     | Data         |例子|
| ------------- |:-------------:|------------- |
| ITMG_MAIN_EVENT_TYPE_ENTER_ROOM    		|result; error_info			|{"error_info":"","result":0}|
| ITMG_MAIN_EVENT_TYPE_EXIT_ROOM    		|result; error_info  			|{"error_info":"","result":0}|
| ITMG_MAIN_EVENT_TYPE_ROOM_DISCONNECT    	|result; error_info  			|{"error_info":"waiting timeout, please check your network","result":0}|
| ITMG_MAIN_EVENT_TYPE_CHANGE_ROOM_TYPE    	|result; error_info; sub_event_type; new_room_type	|{"error_info":"","new_room_type":0,"result":0}|
| ITMG_MAIN_EVENT_TYPE_SPEAKER_NEW_DEVICE	|result; error_info  			|{"deviceID":"{0.0.0.00000000}.{a4f1e8be-49fa-43e2-b8cf-dd00542b47ae}","deviceName":"扬声器 (Realtek High Definition Audio)","error_info":"","isNewDevice":true,"isUsedDevice":false,"result":0}|
| ITMG_MAIN_EVENT_TYPE_SPEAKER_LOST_DEVICE    	|result; error_info  			|{"deviceID":"{0.0.0.00000000}.{a4f1e8be-49fa-43e2-b8cf-dd00542b47ae}","deviceName":"扬声器 (Realtek High Definition Audio)","error_info":"","isNewDevice":false,"isUsedDevice":false,"result":0}|
| ITMG_MAIN_EVENT_TYPE_MIC_NEW_DEVICE    	|result; error_info  			|{"deviceID":"{0.0.1.00000000}.{5fdf1a5b-f42d-4ab2-890a-7e454093f229}","deviceName":"麦克风 (Realtek High Definition Audio)","error_info":"","isNewDevice":true,"isUsedDevice":true,"result":0}|
| ITMG_MAIN_EVENT_TYPE_MIC_LOST_DEVICE    	|result; error_info 			|{"deviceID":"{0.0.1.00000000}.{5fdf1a5b-f42d-4ab2-890a-7e454093f229}","deviceName":"麦克风 (Realtek High Definition Audio)","error_info":"","isNewDevice":false,"isUsedDevice":true,"result":0}|
| ITMG_MAIN_EVNET_TYPE_USER_UPDATE    		|user_list;  event_id			|{"event_id":1,"user_list":["0"]}|
| ITMG_MAIN_EVENT_TYPE_NUMBER_OF_USERS_UPDATE |AllUser; AccUser; ProxyUser |{"AllUser":3,"AccUser":2,"ProxyUser":1}|
| ITMG_MAIN_EVENT_TYPE_NUMBER_OF_AUDIOSTREAMS_UPDATE |AudioStreams |{"AudioStreams":3}|
| ITMG_MAIN_EVENT_TYPE_CHANGE_ROOM_QUALITY |weight; loss; delay |{"weight":5,"loss":0.1,"delay":1}|
| ITMG_MAIN_EVNET_TYPE_PTT_RECORD_COMPLETE 	|result; file_path  			|{"file_path":"","result":0}|
| ITMG_MAIN_EVNET_TYPE_PTT_UPLOAD_COMPLETE 	|result; file_path;file_id  		|{"file_id":"","file_path":"","result":0}|
| ITMG_MAIN_EVNET_TYPE_PTT_DOWNLOAD_COMPLETE	|result; file_path;file_id  		|{"file_id":"","file_path":"","result":0}|
| ITMG_MAIN_EVNET_TYPE_PTT_PLAY_COMPLETE 	|result; file_path  			|{"file_path":"","result":0}|
| ITMG_MAIN_EVNET_TYPE_PTT_SPEECH2TEXT_COMPLETE	|result; text;file_id		|{"file_id":"","text":"","result":0}|
| ITMG_MAIN_EVNET_TYPE_PTT_STREAMINGRECOGNITION_COMPLETE	|result; file_path; text;file_id		|{"file_id":"","file_path":","text":"","result":0}|
