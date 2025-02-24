## TUICallEngine API 简介

TUICallEngine API 是音视频通话组件的**无 UI 接口**，如果 TUICallKit 的交互并不满足您的需求，您可以使用这套接口自己封装交互。

<h2 id="TUICallEngine"> API 概览</h2>

| API | 描述 |
|-----|-----|
| [createInstance](#createInstance) | 创建 TUICallEngine 实例（单例模式）|
| [destroyInstance](#destroyInstance) | 销毁 TUICallEngine 实例（单例模式）|
| [init](#init) | 完成音视频通话基础能力的鉴权|
| [addObserver](#addObserver) | 增加事件回调|
| [removeObserver](#removeObserver) | 移除回调接口|
| [call](#call) | 发起 1v1 通话|
| [groupCall](#groupCall) | 发起群组通话|
| [accept](#accept) | 接听通话 |
| [reject](#reject) | 拒绝通话 |
| [hangup](#hangup) | 结束通话|
| [ignore](#ignore) | 忽略通话|
| [inviteUser](#inviteUser) | 在群组通话中，邀请其他人加入 |
| [joinInGroupCall](#joinInGroupCall) | 主动加入当前的群组通话中 |
| [switchCallMediaType](#switchCallMediaType) | 切换通话媒体类型，比如视频通话切音频通话|
| [setRenderView](#setRenderView) | 设置显示视频画面的 View 对象 |
| [startRemoteView](#startRemoteView) | 设置显示视频画面的 View 对象 |
| [stopRemoteView](#stopRemoteView) | 设置显示视频画面的 View 对象 |
| [openCamera](#opencamera) | 开启摄像头|
| [closeCamara](#closecamara) | 关闭摄像头|
| [switchCamera](#switchcamera) | 切换前后摄像头|
| [openMicrophone](#setmicmute) | 打开麦克风|
| [closeMicrophone](#sethandsfree) | 关闭麦克风|
| [selectAudioPlaybackDevice](#setmicmute) | 选择音频播放设备（听筒/免提）|
| [setSelfInfo](#setSelfInfo) | 设置用户的头像、昵称|
| [enableMultiDeviceAbility](#enableMultiDeviceAbility) | 开启/关闭 TUICallEngine 的多设备登录模式 （尊享版套餐支持）|

<h2 id="TUICallEngine"> API 详情</h2>
### createInstance
创建 TUICallEngine 的单例。
```java
TUICallEngine createInstance(Context context)
```

### destroyInstance
销毁 TUICallEngine 的单例。
```java
void destroyInstance();
```

### init
初始化函数，请在使用所有功能之前先调用该函数，以便完成包含通话服务鉴权在内初始化动作。
```java
void init(String sdkAppID, String userId, String userSig， TUIDefine.Callback callback)
```
参数如下表所示：

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| sdkAppID | int | 您可以在实时音视频控制台 >【[应用管理](https://console.cloud.tencent.com/trtc/app)】> 应用信息中查看 SDKAppID。 |
| userId | String | 当前用户的 ID，字符串类型，只允许包含英文字母（a-z 和 A-Z）、数字（0-9）、连词符（-）和下划线（\_）。 |
| userSig | String | 腾讯云设计的一种安全保护签名，获取方式请参见 [如何计算及使用 UserSig](https://cloud.tencent.com/document/product/647/17275)。 |
| callback | TUIDefine.Callback | 初始化回调，`onSuccess`表示初始化成功。 |

### addObserver
添加回调接口，您可以通过这个接听，监听`TUICallObserver`相关的事件回调。
```java
void addObserver(TUICallObserver observer);
```


### removeListener
移除回调接口。
```java
void removeListener(TRTCVideoCallListener listener);
```

### call
拨打电话（1v1通话）

```java
 void call(TUIRoomId roomId，String userId, TUICallMediaType mediaType, TUICallback callback)
```

参数如下表所示：

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| roomId | TUIRoomId | 此次通话的音视频房间 Id，目前仅支持数字房间号，后续版本会支持字符串房间号 |
| userId | String | 目标用户的userId |
| mediaType | TUICallMediaType | 通话的媒体类型，比如视频通话、语音通话 |

### groupCall
发起群组通话，注意：使用群组通话前需要创建IM 群组，如果已经创建，请忽略；

```java
void groupCall(TUIRoomId roomId，String groupId, List<String> userIdList, TUICallingType callingType，TUICallback callback)
```

| 参数 | 类型 | 含义 |
|-----|-----|-----|
| roomId | TUIRoomId | 此次通话的音视频房间 Id，目前仅支持数字房间号，后续版本会支持字符串房间号 |
| groupId | String | 此次群组通话的群 Id. |
| userIdList | List | 目标用户的userId 列表 |
| mediaType | TUICallMediaType | 通话的媒体类型，比如视频通话、语音通话 |


### accept

接受当前通话，当您作为被叫收到 `onCallReceived()` 的回调时，可以调用该函数接听来电。
```java
void accept(TUIDefine.Callback callback);
```

### reject

拒绝当前通话，当您作为被叫收到 `onCallReceived()` 的回调时，可以调用该函数拒绝来电。
```java
void reject(TUIDefine.Callback callback);
```

### ignore
忽略当前通话，当您作为被叫收到 `onCallReceived()` 的回调时，可以调用该函数忽略来电，此时主叫会收到`onUserLineBusy`的回调；

备注：如果您的业务中存在直播、会议等场景，在直播/会议中的情况时，也可以调用这个函数来忽略此次来电；
```java
void ignore(TUIDefine.Callback callback);
```

### hangup
挂断当前通话，当您处于通话中，可以调用该函数结束通话。

```java
void hangup(TUIDefine.Callback callback);
```

### inviteUser
邀请用户加入此次群组通话，使用场景：一个群组通话中的用户主动邀请其他人时使用。

```java
void inviteUser(List<String> userIdList, TUIDefine.ValueCallback callback);
```

### joinInGroupCall
主动加入此次群组通话，使用场景：群组内用户主动加入此次群组通话使用。

```java
void joinInGroupCall(TUIDefine.RoomId roomId, String groupId, TUICallDefine.MediaType callMediaType, TUIDefine.Callback callback);
```

### switchCallMediaType
切换视频通话到语音通话。

```java
void switchCallMediaType(TUICallDefine.MediaType callMediaType);
```

### setRenderView
视频通话中，给本地和远端用户的设置视频画面显示的View，此接口调用时机如下：
- 本地：在呼叫/收到来电之前，在`openCamera`之前调用即可。
- **远端：在收到`onUserJoin`的回调以后，就可以调用这个接口，设置对应userid的视频渲染View；**

```java
void setRenderView(String userId, TUIVideoView videoView, TUIDefine.Callback callback);
```

### openCamera

开启摄像头。
```java
void openCamera(TUICallDefine.Camera camera, TUIDefine.Callback callback);
```

### closeCamera

关闭摄像头。
```java
void closeCamera();
```

### switchCamera
切换前后摄像头。
```java
void switchCamera(TUICallDefine.Camera camera);
```

### openMicrophone

打开麦克风。
```java
void openMicrophone(TUIDefine.Callback callback);
```

### closeMicrophone
关闭麦克风。
```java
void closeMicrophone();
```

### selectAudioPlaybackDevice

选择音频播放设备，目前支持听筒、扬声器，在通话场景中，可以使用这个接口来开启/关闭免提模式。
```java
void selectAudioPlaybackDevice(TUICallDefine.AudioPlaybackDevice audioPlayType);
```

### setSelfInfo
设置用户头像、昵称的接口。
```java
void setSelfInfo(String nickName, String avatar, TUIDefine.Callback callback);
```

### enableMultiDeviceAbility
开启/关闭 TUICallEngine 的多设备登录模式 （尊享版套餐支持）
```java
void enableMultiDeviceAbility(boolean enable, TUIDefine.Callback callback);
```