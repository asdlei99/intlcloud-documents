`RTCVoiceRoom` has the following features based on Tencent Real-Time Communication (TRTC) and Instant Messaging (IM):

- The anchor can create a voice chat room to start live streaming, and viewers can enter the room to listen/interact.
- The anchor can invite viewers to mic on and kick off mic-on viewers for mic-off.
- The anchor can block seats so that viewers cannot apply for mic-on.
- Viewer can apply for mic-on to become anchor and interact with other viewers or mic off to become viewer.
- All users can send various text and custom messages. Custom messages can be used to send on-screen comments, give likes, and give gifts.

`TRTCVoiceRoom` is an open-source class depending on two closed-source Tencent Cloud SDKs. For the specific implementation process, please see [iOS](https://intl.cloud.tencent.com/document/product/647/37287).

- TRTC SDK: the [TRTC SDK](https://intl.cloud.tencent.com/document/product/647) is used as the low-latency voice chat component.
- IM SDK: the `AVChatRoom` feature of the [IM SDK](https://intl.cloud.tencent.com/document/product/1047) is used to implement chat rooms. In addition, the attribute APIs of IM is used to store room information such as the seat list, and invitation signaling can be used to apply for mic-on/pick.


<h2 id="TRTCVoiceRoom">TRTCVoiceRoom API Overview</h2>

### Basic SDK APIs

| API | Description |
| ----------------------------------------------- | ------------------------ |
| [sharedInstance](#sharedinstance) | Gets singleton object.           |
| [destroySharedInstance](#destroysharedinstance) | Terminates singleton object. |
| [setDelegate](#setdelegate) | Sets event callback. |
| [setDelegateHandler](#setdelegatehandler) | Sets the thread where the event callback is. |
| [login](#login)                   | Logs in.                   |
| [logout](#logout)                 | Logs out.                   |
| [setSelfProfile](#setselfprofile) | Modifies personal information. |

### Room APIs

| API | Description |
| ----------------------------------- | ------------------------------------------------------------ |
| [createRoom](#createroom) | Creates room (called by anchor). If the room does not exist, the system will automatically create a room. |
| [destroyRoom](#destroyroom) | Terminates room (called by anchor). |
| [enterRoom](#enterroom) | Enters room (called by viewer). |
| [exitRoom](#exitroom) | Exits room (called by viewer). |
| [getRoomInfoList](#getroominfolist) | Gets room list details.                                     |
| [getUserInfoList](#getuserinfolist) | Gets the user information of the specified `userId`. If the value is `null`, the information of all users in the room will be obtained. |

### Seat management APIs

| API | Description |
| ----------------------- | ----------------------------------- |
| [enterSeat](#enterseat) | Actively mics on (called by anchor or viewer).    |
| [leaveSeat](#leaveseat) | Actively mics off (called by anchor or viewer).    |
| [pickSeat](#pickseat)   | Picks viewer for mic-on (called by anchor).                  |
| [kickSeat](#kickseat)   | Kicks off viewer for mic-off (called by anchor).                  |
| [muteSeat](#muteseat)   | Mutes/Unmutes seat (called by anchor). |
| [closeSeat](#closeseat) | Blocks/Unblocks seat (called by anchor).          |

### Local audio operation APIs

| API | Description |
| ----------------------------------------------- | -------------------- |
| [startMicrophone](#startmicrophone)             | Enables mic capturing.     |
| [stopMicrophone](#stopmicrophone)               | Stops mic capturing.     |
| [setAudioQuality](#setaudioquality)             | Sets audio quality.           |
| [muteLocalAudio](#mutelocalaudio)               | Mutes/Unmutes local audio.       |
| [setSpeaker](#setspeaker)                       | Enables speaker.     |
| [setAudioCaptureVolume](#setaudiocapturevolume) | Sets mic capturing volume level. |
| [setAudioPlayoutVolume](#setaudioplayoutvolume) | Sets playback volume level.       |


### Remote user audio operation APIs

| API | Description |
| ----------------------------------------- | ----------------------- |
| [muteRemoteAudio](#muteremoteaudio)       | Mutes/Unmutes specified member. |
| [muteAllRemoteAudio](#muteallremoteaudio) | Mutes/Unmutes all members. |

### Background music and sound effect APIs

| API | Description |
| ----------------------------------------------- | ------------------------------------------------------------ |
| [getAudioEffectManager](#getaudioeffectmanager) | Gets background music and sound effect management object [TXAudioEffectManager](#trtcaudioeffectmanagerapi). |

### Message sending APIs

| API | Description |
| --------------------------------------- | ---------------------------------------- |
| [sendRoomTextMsg](#sendroomtextmsg) | Broadcasts text message in room. This API is generally used for on-screen comment chat. |
| [sendRoomCustomMsg](#sendroomcustommsg) | Sends custom text message. |

### Invitation signaling APIs

| API | Description |
| ------------------------------------- | ---------------- |
| [sendInvitation](#sendinvitation)     | Sends invitation to user. |
| [acceptInvitation](#acceptinvitation) | Accepts invitation.       |
| [rejectInvitation](#rejectinvitation) | Declines invitation.       |
| [cancelInvitation](#cancelinvitation) | Cancels invitation.       |

<h2 id="TRTCVoiceRoomDelegate">TRTCVoiceRoomDelegate API Overview</h2>

### General event callbacks

| API | Description |
| ------------------------- | ---------- |
| [onError](#onerror) | Callback for error. |
| [onWarning](#onwarning) | Callback for warning. |
| [onDebugLog](#ondebuglog) | Callback for log. |

### Room event callbacks

| API | Description |
| ----------------------------------------- | ---------------------- |
| [onRoomDestroy](#onroomdestroy) | Callback for room termination. |
| [onRoomInfoChange](#onroominfochange)     | Callback for voice chat room information change. |
| [onUserVolumeUpdate](#onuservolumeupdate) | Callback for user call volume level.     |

### Seat change callbacks

| API | Description |
| --------------------------------------- | ------------------------------------- |
| [onSeatListChange](#onseatlistchange)   | The full seat list changed.                |
| [onAnchorEnterSeat](#onanchorenterseat) | A member miced on (actively or picked by the anchor). |
| [onAnchorLeaveSeat](#onanchorleaveseat) | A member miced off (actively or kicked off by the anchor). |
| [onSeatMute](#onseatmute)               | The anchor muted a seat.                          |
| [onSeatClose](#onseatclose)             | The anchor blocked a seat.                          |

### Viewer room entry/exit event callbacks

| API | Description |
| ----------------------------------- | ------------------ |
| [onAudienceEnter](#onaudienceenter) | Notification of viewer's room entry. |
| [onAudienceExit](#onaudienceexit) | Notification of viewer's room exit. |

### Message event callbacks

| API | Description |
| ------------------------------------------- | ---------------- |
| [onRecvRoomTextMsg](#onrecvroomtextmsg)     | Receipt of text message.   |
| [onRecvRoomCustomMsg](#onrecvroomcustommsg) | Receipt of custom message. |

### Signaling event callbacks

| API | Description |
| ------------------------------------------------- | ------------------ |
| [onReceiveNewInvitation](#onreceivenewinvitation) | A new invitation was received.   |
| [onInviteeAccepted](#oninviteeaccepted)           | The invitee accepted the invitation.   |
| [onInviteeRejected](#oninviteerejected)           |  The invitee declined the invitation.   |
| [onInvitationCancelled](#oninvitationcancelled)   | The inviter canceled the invitation. |

## Basic SDK APIs

<span id="sharedInstance"></span>

### sharedInstance

This API is used to get the [TRTCVoiceRoom](https://intl.cloud.tencent.com/document/product/647/37286) singleton object.

```Objective-C
/**
* This API is used to get the `TRTCVoiceRoom` singleton object
*
* - returns: `TRTCVoiceRoom` instance
* - note: `{@link TRTCVoiceRoom#destroySharedInstance()}` can be called to terminate a singleton object
*/
+ (instancetype)sharedInstance NS_SWIFT_NAME(shared());
```


### destroySharedInstance

This API is used to terminate the [TRTCVoiceRoom](https://intl.cloud.tencent.com/document/product/647/37287) singleton object.

>?After the instance is terminated, the externally cached `TRTCVoiceRoom` instance cannot be used, and you need to call [sharedInstance](#sharedInstance) again to get a new instance.

```Objective-C
/**
* This API is used to terminate the `TRTCVoiceRoom` singleton object
*
* - note: after the instance is terminated, the externally cached `TRTCVoiceRoom` instance cannot be reused, and you need to call `{@link TRTCVoiceRoom#sharedInstance()}` again to get a new instance
*/
+ (void)destroySharedInstance NS_SWIFT_NAME(destroyShared());
```

### setDelegate

This API is used to get the event callback of [TRTCVoiceRoom](https://intl.cloud.tencent.com/document/product/647/37287). You can use `TRTCVoiceRoomDelegate` to get various status notifications of [TRTCVoiceRoom](https://intl.cloud.tencent.com/document/product/647/37286).

```Objective-C
/**
* This API is used to set the component callback
* 
* You can use `TRTCVoiceRoomDelegate` to get various status notifications of `TRTCVoiceRoom`
*
* - parameter delegate   Callback API
* - note: callback events in `TRTCVoiceRoom` are called back to you in the main queue by default. If you need to specify a queue for event callback, please use `{@link TRTCVoiceRoom#setDelegateQueue(queue)}`
*/
- (void)setDelegate:(id<TRTCVoiceRoomDelegate>)delegate NS_SWIFT_NAME(setDelegate(delegate:));
```

>?`setDelegate` is the delegation callback of `TRTCVoiceRoom`.   

### setDelegateQueue

This API is used to set the thread queue where the event callback is. The main thread (MainQueue) is used by default.

```Objective-C
/**
* This API is used to set the queue where the event callback is
*
* - parameter queue   Queue. Various status callback notifications in `TRTCVoiceRoom` will be sent to the queue you specify
*/
- (void)setDelegateQueue:(dispatch_queue_t)queue NS_SWIFT_NAME(setDelegateQueue(queue:));

```

The parameters are as detailed below:

| Parameter | Type | Description |
| ----- | ---------------- | ------------------------------------------------------------ |
| queue | dispatch_queue_t | Various status notifications in `TRTCVoiceRoom` will be sent to the thread queue you specify. |

   

### login

This API is used to log in.

```Objective-C
- (void)login:(int)sdkAppID
       userId:(NSString *)userId
      userSig:(NSString *)userSig
     callback:(ActionCallback _Nullable)callback NS_SWIFT_NAME(login(sdkAppID:userId:userSig:callback:));

```

The parameters are as detailed below:

| Parameter | Type | Description |
| -------- | -------------- | ------------------------------------------------------------ |
| sdkAppId | int | You can view the `SDKAppID` in the TRTC Console > **[Application Management](https://console.cloud.tencent.com/trtc/app)** > "Application Info". |
| userId | String | ID of current user, which is a string that can contain only letters (a–z and A–Z), digits (0–9), hyphens (-), and underscores (\_). |
| userSig | String | Tencent Cloud's proprietary security protection signature. For more information on how to get it, please see [How to Calculate UserSig](https://intl.cloud.tencent.com/document/product/647/35166). |
| callback | ActionCallback | Callback for login. The `code` will be 0 if the operation succeeds. |

   

### logout

This API is used to log out.

```Objective-C
- (void)logout:(ActionCallback _Nullable)callback NS_SWIFT_NAME(logout(callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| -------- | -------------- | --------------------------- |
| callback | ActionCallback | Callback for logout. The `code` will be 0 if the operation succeeds. |

   

### setSelfProfile

This API is used to modify the personal information.

```Objective-C
- (void)setSelfProfile:(NSString *)userName avatarURL:(NSString *)avatarURL callback:(ActionCallback _Nullable)callback NS_SWIFT_NAME(setSelfProfile(userName:avatarURL:callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| --------- | -------------- | ----------------------------------- |
| userName | String | Nickname. |
| avatarURL | String | Profile photo address. |
| callback | ActionCallback | Callback for personal information setting. The `code` will be 0 if the operation succeeds. |

   


## Room APIs

### createRoom

This API is used to create a room (called by the anchor).

```Objective-C
- (void)createRoom:(int)roomID roomParam:(VoiceRoomParam *)roomParam callback:(ActionCallback _Nullable)callback NS_SWIFT_NAME(createRoom(roomID:roomParam:callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| --------- | ------------------- | ------------------------------------------------------------ |
| roomId | int | Room ID. You need to assign and manage the IDs in a centralized manner. Multiple `roomID` values can be aggregated as a voice chat room list. Currently, Tencent Cloud does not provide the voice chat room list management service. Please manage your voice chat room list on your own. |
| roomParam | TRTCCreateRoomParam | Room description information, such as room name, seat information, and cover information. To manage seats, you must enter the number of seats in the room. |
| callback | ActionCallback | Callback for room creation result. The `code` will be 0 if the operation succeeds.  |

Generally, the anchor starts live streaming in the following call process: 
1. The anchor calls `createRoom` to create a voice chat room. At this time, room attribute information such as the room ID, whether mic-on needs confirmation by the anchor, and the number of seats is passed in.
2. After successfully creating the room, the anchor calls `enterSeat` to enter the seat.
3. The anchor receives the `onSeatListChange` seat list change event notification from the component. At this time, the seat list change can be refreshed and displayed on the UI.
4. The anchor will also receive the `onAnchorEnterSeat` event notification of that a member entered the seat list. At this time, mic capturing will be automatically enabled.

   

### destroyRoom

This API is used to terminate a room (called by the anchor). After creating a room, the anchor can call this API to terminate it.

```Objective-C
- (void)destroyRoom:(ActionCallback _Nullable)callback NS_SWIFT_NAME(destroyRoom(callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| -------- | -------------- | ------------------------------------- |
| callback | ActionCallback | Callback for room termination result. The `code` will be 0 if the operation succeeds. |


### enterRoom

This API is used to enter a room (called by the viewer).

```Objective-C
- (void)enterRoom:(NSInteger)roomID callback:(ActionCallback _Nullable)callback NS_SWIFT_NAME(enterRoom(roomID:callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| -------- | -------------- | ------------------------------------- |
| roomId | int | Room ID. |
| callback | ActionCallback | Callback for room entry result. The `code` will be 0 if the operation succeeds.  |


Generally, the viewer can enter a room and listen in the following call process: 

1. The viewer gets the latest voice chat room list from your server. The list may contain `roomId` and room information of multiple voice chat rooms.
2. The viewer selects a voice chat room and calls `enterRoom` and passes in the room ID to enter the room.
3. After room entry, the component's `onRoomInfoChange` room attribute change event notification will be received. At this time, the room attributes can be recorded, and corresponding changes can be made, such as the room name displayed on the UI and whether mic-on requires approval by the anchor.
4. After room entry, the `onSeatListChange` seat list change event notification will be received from the component. At this time, the seat list change can be refreshed and displayed on the UI.
5. After room entry, the `onAnchorEnterSeat` event notification that the anchor entered the seat list will also be received.

### exitRoom

This API is used to exit a room.

```Objective-C
- (void)exitRoom:(ActionCallback _Nullable)callback NS_SWIFT_NAME(exitRoom(callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| -------- | -------------- | ------------------------------------- |
| callback | ActionCallback | Callback for room exit result. The `code` will be 0 if the operation succeeds. |

   

### getRoomInfoList

This API is used to get the room list details where the room name and cover are set through `roomInfo` by the anchor during `createRoom()`.

>?If both the room list and room information are managed on your server, you can ignore this parameter.


```Objective-C
- (void)getRoomInfoList:(NSArray<NSNumber *> *)roomIdList callback:(VoiceRoomInfoCallback _Nullable)callback NS_SWIFT_NAME(getRoomInfoList(roomIdList:callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ---------- | ------------------- | ------------------ |
| roomIdList | List&lt;Integer&gt; | Room ID list. |
| callback | RoomInfoCallback | Callback for room details. |


### getUserInfoList

This API is used to get the user information of a specified `userId`.

```Objective-C
- (void)getUserInfoList:(NSArray<NSString *> * _Nullable)userIDList callback:(VoiceRoomUserListCallback _Nullable)callback NS_SWIFT_NAME(getUserInfoList(userIDList:callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ---------------- | ------------------ | ------------------------------------------------------------ |
| userIdList       | List&lt;String&gt; | List of user IDs to be obtained. If this parameter is null, the information of all users in the room will be obtained. |
| userlistcallback | UserListCallback   | Callback for user details.                                           |


## Seat Management APIs

### enterSeat

This API is used to actively mic on (called by anchor or viewer).

>?After successful mic-on, all members in the room will receive the event notifications of `onSeatListChange` and `onAnchorEnterSeat`.

```Objective-C
- (void)enterSeat:(NSInteger)seatIndex callback:(ActionCallback _Nullable)callback NS_SWIFT_NAME(enterSeat(seatIndex:callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| --------- | -------------- | -------------------- |
| seatIndex | int            | Seat number for mic-on. |
| callback | ActionCallback | Callback for operation. |

Calling this API will immediately modify the seat list. In the scenario where the viewer needs to submit an application to the anchor, the viewer can call `sendInvitation` first to apply to the anchor and call this API after receiving `onInvitationAccept`.

### leaveSeat

This API is used to actively mic off (called by anchor or viewer).

>? After successful mic-off, all members in the room will receive the event notifications of `onSeatListChange` and `onAnchorLeaveSeat`.

```Objective-C
- (void)leaveSeat:(ActionCallback _Nullable)callback NS_SWIFT_NAME(leaveSeat(callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| -------- | -------------- | ---------- |
| callback | ActionCallback | Callback for operation. |

### pickSeat

This API is used to pick a viewer for mic-on (called by anchor).

>? After the anchor picks the viewer for mic-on, all members in the room will receive event notifications of `onSeatListChange` and `onAnchorEnterSeat`.

```Objective-C
- (void)pickSeat:(NSInteger)seatIndex userId:(NSString *)userId callback:(ActionCallback _Nullable)callback NS_SWIFT_NAME(pickSeat(seatIndex:userId:callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| --------- | -------------- | ---------------------- |
| seatIndex | int            | Seat number for picked mic-on. |
| userId | String | User ID. |
| callback | ActionCallback | Callback for operation. |

Calling this API will immediately modify the seat list. In the scenario where the anchor can mic on only if approved by the viewer, the anchor can call `sendInvitation` first to apply to the viewer and call this API after receiving `onInvitationAccept`.


### kickSeat

This API is used to kick off a viewer for mic-off (called by anchor).

>? After the anchor kicks off the viewer for mic-off, all members in the room will receive the event notifications of `onSeatListChange` and `onAnchorLeaveSeat`.

```Objective-C
- (void)kickSeat:(NSInteger)seatIndex callback:(ActionCallback _Nullable)callback NS_SWIFT_NAME(kickSeat(seatIndex:callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| --------- | -------------- | ---------------------- |
| seatIndex | int            | Seat number for kicked mic-off. |
| callback | ActionCallback | Callback for operation. |

Calling this API will immediately modify the seat list. In the scenario where the anchor can mic on only if approved by the viewer, the anchor can call `sendInvitation` first to apply to the viewer and call this API after receiving `onInvitationAccept`.

### muteSeat

This API is used to mute/unmute a seat (called by anchor).

>? After a seat is muted/unmuted, all members in the room will receive the event notifications of `onSeatListChange` and `onSeatMute`.

```Objective-C
- (void)muteSeat:(NSInteger)seatIndex isMute:(BOOL)isMute callback:(ActionCallback _Nullable)callback NS_SWIFT_NAME(muteSeat(seatIndex:isMute:callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| --------- | -------------- | --------------------------------------------- |
| seatIndex | int            | Seat number for operation.                          |
| isMute    | boolean        | true: mutes seat; false: unmutes seat. |
| callback | ActionCallback | Callback for operation. |

Calling this API will immediately modify the seat list. The anchor on the corresponding seat `seatIndex` will automatically call `muteAudio` to mute/unmute.

### closeSeat

This API is used to block/unblock a seat (called by anchor).

>? After the anchor blocks/unblocks the seat, all members in the room will receive the event notifications of `onSeatListChange` and `onSeatClose`.

```Objective-C
- (void)closeSeat:(NSInteger)seatIndex isClose:(BOOL)isClose callback:(ActionCallback _Nullable)callback NS_SWIFT_NAME(closeSeat(seatIndex:isClose:callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| --------- | -------------- | ------------------------------------------ |
| seatIndex | int            | Seat number for operation.                          |
| isClose   | boolean        | true: blocks seat; false: unblocks seat. |
| callback | ActionCallback | Callback for operation. |

Calling this API will immediately modify the seat list. The anchor on the corresponding seat `seatIndex` will automatically mic off.


## Local Audio Operation APIs

### startMicrophone

This API is used to enable mic capturing.

```Objective-C
- (void)startMicrophone;
```

### stopMicrophone

This API is used to stop mic capturing.

```Objective-C
- (void)stopMicrophone;
```

### setAudioQuality

This API is used to set the audio quality.

```Objective-C
- (void)setAuidoQuality:(NSInteger)quality NS_SWIFT_NAME(setAuidoQuality(quality:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ------- | ---- | ------------------------------------------------------------ |
| quality | int | Audio quality. For more information, please see [TRTC SDK](http://doc.qcloudtrtc.com/group__TRTCCloud__android.html#a955cccaddccb0c993351c656067bee55). |


### muteLocalAudio

This API is used to mute/unmute the local audio.

```Objective-C
- (void)muteLocalAudio:(BOOL)mute NS_SWIFT_NAME(muteLocalAudio(mute:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ---- | ------- | ------------------------------------------------------------ |
| mute | boolean | Mutes/Unmutes. For more information, please see [TRTC SDK](http://doc.qcloudtrtc.com/group__TRTCCloud__android.html#a37f52481d24fa0f50842d3d8cc380d86). |



### setSpeaker

This API is used to enable the speaker.

```Objective-C
- (void)setSpeaker:(BOOL)userSpeaker NS_SWIFT_NAME(setSpeaker(userSpeaker:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ---------- | ------- | --------------------------- |
| useSpeaker | boolean | true: speaker; false: receiver. |



### setAudioCaptureVolume

This API is used to set the mic capturing volume level.

```Objective-C
- (void)setAudioCaptureVolume:(NSInteger)voluem NS_SWIFT_NAME(setAudioCaptureVolume(volume:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ------ | ---- | ----------------------------- |
| volume | int | Capture volume level. Value range: 0–100. Default value: 100. |


### setAudioPlayoutVolume

This API is used to set the playback volume level.

```Objective-C
- (void)setAudioPlayoutVolume:(NSInteger)volume NS_SWIFT_NAME(setAudioPlayoutVolume(volume:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ------ | ---- | ----------------------------- |
| volume | int | Playback volume level. Value range: 0–100. Default value: 100. |

### muteRemoteAudio

This API is used to mute/unmute a specified member.

```Objective-C
- (void)muteRemoteAudio:(NSString *)userId mute:(BOOL)mute NS_SWIFT_NAME(muteRemoteAudio(userId:mute:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ------ | ------- | --------------------------------- |
| userId | String | Specified user ID. |
| mute | boolean | true: mutes; false: unmutes. |

### muteAllRemoteAudio

This API is used to mute/unmute all members.

```Objective-C
- (void)muteAllRemoteAudio:(BOOL)isMute NS_SWIFT_NAME(muteAllRemoteAudio(isMute:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ---- | ------- | --------------------------------- |
| mute | boolean | true: mutes; false: unmutes. |

   

## Background Music and Sound Effect APIs

### getAudioEffectManager

This API is used to get the background music and sound effect management object [TXAudioEffectManager](http://doc.qcloudtrtc.com/group__TRTCCloud__android.html#a3646dad993287c3a1a38a5bc0e6e33aa).

```Objective-C
- (TXAudioEffectManager * _Nullable)getAudioEffectManager;
```


## Message Sending APIs

### sendRoomTextMsg

This API is used to broadcast a text message in the room, which is generally used for on-screen comment chat.

```Objective-C
- (void)sendRoomTextMsg:(NSString *)message callback:(ActionCallback _Nullable)callback NS_SWIFT_NAME(sendRoomTextMsg(message:callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| -------- | -------------- | -------------- |
| message | String | Text message. |
| callback | ActionCallback | Callback for sending result. |

   

### sendRoomCustomMsg

This API is used to send a custom text message.

```Objective-C
- (void)sendRoomCustomMsg:(NSString *)cmd message:(NSString *)message callback:(ActionCallback _Nullable)callback NS_SWIFT_NAME(sendRoomCustomMsg(cmd:message:callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| -------- | -------------- | -------------------------------------------------- |
| cmd | String | Custom command word used to distinguish between different message types. |
| message | String | Text message. |
| callback | ActionCallback | Callback for sending result. |

   

## Invitation Signaling APIs

### sendInvitation

This API is used to send an invitation to a user.

```Objective-C
- (NSString *)sendInvitation:(NSString *)cmd
                      userId:(NSString *)userId
                     content:(NSString *)content
                    callback:(ActionCallback _Nullable)callback NS_SWIFT_NAME(sendInvitation(cmd:userId:content:callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| -------- | -------------- | ---------------- |
| cmd      | String         | Custom command of business. |
| userId   | String         | Invitee user ID.  |
| content  | String         | Invitation content.     |
| callback | ActionCallback | Callback for sending result. |

Returned value:

| Returned Value | Type | Description |
| -------- | ------ | --------------------- |
| inviteId | String | Invitation ID. |

### acceptInvitation

This API is used to accept an invitation.

```Objective-C
- (void)acceptInvitation:(NSString *)identifier callback:(ActionCallback _Nullable)callback NS_SWIFT_NAME(acceptInvitation(identifier:callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| -------- | -------------- | -------------- |
| id       | String         | Invitation ID.      |
| callback | ActionCallback | Callback for sending result. |

### rejectInvitation

This API is used to decline an invitation.

```Objective-C
- (void)rejectInvitation:(NSString *)identifier callback:(ActionCallback _Nullable)callback NS_SWIFT_NAME(rejectInvitation(identifier:callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| -------- | -------------- | -------------- |
| id       | String         | Invitation ID.      |
| callback | ActionCallback | Callback for sending result. |


### cancelInvitation

This API is used to cancel an invitation.

```Objective-C
- (void)cancelInvitation:(NSString *)identifier callback:(ActionCallback _Nullable)callback NS_SWIFT_NAME(cancelInvitation(identifier:callback:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| -------- | -------------- | -------------- |
| id       | String         | Invitation ID.      |
| callback | ActionCallback | Callback for sending result. |

## TRTCVoiceRoomDelegate Event Callbacks

## General Event Callbacks

### onError

Callback for error.

>? The SDK encountered an irrecoverable error and must be listened on. Corresponding UI reminders should be displayed based on the actual conditions.

```Objective-C
- (void)onError:(int)code
                message:(NSString*)message
NS_SWIFT_NAME(onError(code:message:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ------- | ------ | ---------- |
| code | int | Error code. |
| message | String | Error message. |


### onWarning

Callback for warning.

```Objective-C
- (void)onWarning:(int)code
                  message:(NSString *)message
NS_SWIFT_NAME(onWarning(code:message:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ------- | ------ | ---------- |
| code | int | Error code. |
| message | String | Warning message. |

   

### onDebugLog

Callback for log.

```Objective-C
- (void)onDebugLog:(NSString *)message
NS_SWIFT_NAME(onDebugLog(message:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ------- | ------ | ---------- |
| message | String | Log message. |

   


## Room Event Callbacks

### onRoomDestroy

Callback for room termination. When the anchor dismisses the room, all users in the room will receive this callback.

```Objective-C
- (void)onRoomDestroy:(NSString *)message
NS_SWIFT_NAME(onRoomDestroy(message:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ------ | ------ | --------- |
| roomId | String | Room ID. |


### onRoomInfoChange

This API will be called back after successful room entry. `roomInfo` contains the information of the created room.

```Objective-C
- (void)onRoomInfoChange:(VoiceRoomInfo *)roomInfo
NS_SWIFT_NAME(onRoomInfoChange(roomInfo:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| -------- | -------- | ---------- |
| roomInfo | RoomInfo | Room information. |

   

### onUserVolumeUpdate

Notification to all members of the volume level after the volume level reminder is enabled.

```Objective-C
- (void)onUserVolumeUpdate:(NSString *)userId
                              volume:(NSInteger)volume
NS_SWIFT_NAME(onUserVolumeUpdate(userId:volume:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ------ | ------ | ------------------------- |
| userId | String | User ID. |
| volume | int | Volume level. Value range: 0–100. |


## Seat Callbacks

### onSeatListChange

The full seat list changed, including the entire seat list.

```Objective-C
- (void)onSeatInfoChange:(NSArray<VoiceRoomSeatInfo *> *)seatInfolist
NS_SWIFT_NAME(onSeatListChange(seatInfoList:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ------------ | -------------------- | ---------------- |
| seatInfoList | List&lt;SeatInfo&gt; | Full seat list. |

### onAnchorEnterSeat

A member miced on (actively or picked by the anchor).

```Objective-C
- (void)onAnchorEnterSeat:(NSInteger)index
                              user:(VoiceRoomUserInfo *)user
NS_SWIFT_NAME(onAnchorEnterSeat(index:user:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ----- | -------- | -------------------- |
| index | int      | Mic-on seat.     |
| user  | UserInfo | Details of the user who mics on. |

### onAnchorLeaveSeat

A member miced off (actively or kicked off by the anchor).

```Objective-C
- (void)onAnchorLeaveSeat:(NSInteger)index
                     user:(VoiceRoomUserInfo *)user
NS_SWIFT_NAME(onAnchorLeaveSeat(index:user:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ----- | -------- | -------------------- |
| index | int      | Mic-off seat.         |
| user  | UserInfo | Details of the user who mics off. |

### onSeatMute

The anchor muted a seat.

```Objective-C
- (void)onSeatMute:(NSInteger)index
            isMute:(BOOL)isMute
NS_SWIFT_NAME(onSeatMute(index:isMute:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ------ | ------- | ---------------------------------- |
| index  | int     | Manipulated seat.                       |
| isMute | boolean | true: mutes seat; false: unmutes seat. |

### onSeatClose

The anchor blocked a seat.

```Objective-C
- (void)onSeatClose:(NSInteger)index
            isClose:(BOOL)isClose
NS_SWIFT_NAME(onSeatClose(index:isClose:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ------- | ------- | ----------------------------------- |
| index   | int     | Manipulated seat.                        |
| isClose | boolean | true: blocks seat; false: unblocks seat. |

## Viewer Room Entry/Exit Event Callbacks

### onAudienceEnter

Notification of viewer's room entry.

```Objective-C
- (void)onAudienceEnter:(VoiceRoomUserInfo *)userInfo
NS_SWIFT_NAME(onAudienceEnter(userInfo:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| -------- | -------- | -------------- |
| userInfo | UserInfo | Information of the viewer who enters the room. |

### onAudienceExit

Notification of viewer's room exit.

```Objective-C
- (void)onAudienceExit:(VoiceRoomUserInfo *)userInfo
NS_SWIFT_NAME(onAudienceExit(userInfo:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| -------- | -------- | -------------- |
| userInfo | UserInfo | Information of the viewer who exits the room. |

   

## Message Event Callbacks

### onRecvRoomTextMsg

Receipt of text message.

```Objective-C
- (void)onRecvRoomTextMsg:(NSString *)message
                 userInfo:(VoiceRoomUserInfo *)userInfo
NS_SWIFT_NAME(onRecvRoomTextMsg(message:userInfo:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| -------- | -------- | ---------------- |
| message | String | Text message. |
| userInfo | UserInfo | User information of sender. |

   

### onRecvRoomCustomMsg

Receipt of custom message.

```Objective-C
- (void)onRecvRoomCustomMsg:(NSString *)cmd
                    message:(NSString *)message
                   userInfo:(VoiceRoomUserInfo *)userInfo
NS_SWIFT_NAME(onRecvRoomCustomMsg(cmd:message:userInfo:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| -------- | -------- | -------------------------------------------------- |
| command | String | Custom command word used to distinguish between different message types. |
| message | String | Text message. |
| userInfo | UserInfo | User information of sender.                                   |

## Invitation Signaling Event Callbacks

### onReceiveNewInvitation

A new invitation was received.

```Objective-C
- (void)onReceiveNewInvitation:(NSString *)identifier
                       inviter:(NSString *)inviter
                           cmd:(NSString *)cmd
                       content:(NSString *)content
NS_SWIFT_NAME(onReceiveNewInvitation(identifier:inviter:cmd:content:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ------- | -------- | ---------------------------------- |
| id      | String   | Invitation ID.                          |
| inviter | String   | Inviter user ID.                  |
| cmd     | String   | Custom command word specified by business. |
| content | UserInfo | Content specified by business.                   |

### onInviteeAccepted

The invitee accepted the invitation.

```Objective-C
- (void)onInviteeAccepted:(NSString *)identifier
                  invitee:(NSString *)invitee
NS_SWIFT_NAME(onInviteeAccepted(identifier:invitee:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ------- | ------ | ------------------- |
| id      | String | Invitation ID.           |
| invitee | String | Invitee ID. |

### onInviteeRejected

The invitee declined the invitation.

```Objective-C
- (void)onInviteeRejected:(NSString *)identifier
                  invitee:(NSString *)invitee
NS_SWIFT_NAME(onInviteeRejected(identifier:invitee:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ------- | ------ | ------------------- |
| id      | String | Invitation ID.           |
| invitee | String | Invitee ID. |

### onInvitationCancelled

The inviter canceled the invitation.

```Objective-C
- (void)onInvitationCancelled:(NSString *)identifier
                      invitee:(NSString *)invitee NS_SWIFT_NAME(onInvitationCancelled(identifier:invitee:));
```

The parameters are as detailed below:

| Parameter | Type | Description |
| ------- | ------ | ----------------- |
| id      | String | Invitation ID.         |
| inviter | String | Inviter user ID. |
