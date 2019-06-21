## 一、概述

### 简介

### Demo体验

请根据需求选择渠道安装，安装完会议Demo后，可体验多人音视频会议功能。

- [iOS Demo下载](https://www.pgyer.com/xoTQ)

- [Android Demo下载](https://www.pgyer.com/eU0U)

- [Web Demo 体验](https://demos.anyrtc.io/ar-meet/)

### 源码GitHub

源码仅供开发者参考，适用于SDK调试，便于快速集成。

- [iOS Demo 源码下载](https://github.com/AnyRTC/anyRTC-Meeting-iOS)

- [Android Demo 源码下载](https://github.com/AnyRTC/anyRTC-Meeting-Android)

- [Web Demo 源码下载](https://github.com/anyRTC/anyRTC-Meeting-Web)
- 
## 二、集成指南

### 适用范围

本集成文档适用于Android ARMeet SDK 3.0.0版本。

### 准备环境

- Android Studio 2.1或以上版本
- Android 版本不低于 4.0.3 且支持音视频的 Android 设备（不支持模拟器）
- Android 设备已经连接到有效网络

### 导入SDK

**Gradle方式导入）**[ ![Download](https://api.bintray.com/packages/dyncanyrtc/ar_dev/meet/images/download.svg) ](https://bintray.com/dyncanyrtc/ar_dev/meet/_latestVersion)

```
dependencies {
  compile 'org.ar:meet_kit:3.0.1'
}

```

### 权限说明

使用ARMeet SDK需以下权限

```
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```
### 混淆配置
为了避免混淆SDK，在Proguard混淆文件中增加以下配置：

```
-dontwarn org.anyrtc.**
-keep class org.anyrtc.**{*;}
-dontwarn org.ar.**
-keep class org.ar.**{*;}
-dontwarn org.webrtc.**
-keep class org.webrtc.**{*;}
```
---
## 三、API接口文档

### ARMeetEngine 类

### 1. 初始化并配置开发者信息

**定义**

```
void initEngine(Context context, String appId, String token)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
context | Context | 上下文对象
appId | String | appId
token | String  | token

**说明**

该方法为配置开发者信息，上述参数均可在https://www.anyrtc.io/ 应用管理中获得；建议在Application调用。

### 2. 配置私有云

**定义**

```
void configServerForPriCloud(String address,int port)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
address | String | 私有云服务地址
port | int | 私有云服务端口

**说明**

配置私有云信息，当使用私有云时才需要进行配置，默认无需配置。

### 3. 获取SDK版本号

**定义**

```
String getSdkVersion()
```
**返回值**

SDK版本号

### 4. 关闭硬解码(安卓特有)

**定义**

```
void disableHWDecode()
```

### 5. 关闭硬编码(安卓特有)

**定义**

```
void disableHWEncode()
```

### 6. 设置日志显示级别

**定义**

```
void setLogLevel(ARLogLevel logLevel) 
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
logLevel | ARLogLevel | 日志显示级别
---

### ARMeetOption配置类

### 1. 获取配置类

**定义**

```
ARMeetOption arMeetOption = ARMeetEngine.Inst().getARMeetOption();
```
### 2. 设置可配置参数

**定义**
```
setOptionParams(boolean isDefaultFrontCamera, ARVideoCommon.ARVideoOrientation mScreenOriention, ARVideoCommon.ARVideoProfile videoProfile,ARVideoCommon.ARVideoFrameRate videoFps, ARVideoCommon.ARMediaType mediaType, ARMeetType meetType, boolean isHost)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
isDefaultFrontCamera | boolean | 是否默认前置摄像头 true 前置 false 后置  默认true
videoOrientation | ARVideoOrientation |视频方向 默认竖直
videoProfile | ARVideoProfile | 视频分辨率  默认360x640
videoFps | ARVideoFrameRate |视频帧率  默认 Fps15
mediaType | ARMediaType |发布媒体类型 Video音视频 Audio 音频 默认音视频
meetType|ARMeetType|会议类型 默认普通模式
isHost|boolean|是否是主持人 默认false (主持人模式下生效)

**说明**  

可通过上面方法配置，也可单独设置

---

### ARMeetKit 类

### 1. 实例化ARMeetKit对象

**定义**

```
ARMeetKit meetKit = new ARMeetKit(ARMeetEvent arMeetEvent);

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
arMeetEvent | ARMeetEvent | 会议回调实现类


### 2. 设置本地视频采集窗口

**定义**

```
int setLocalVideoCapturer(long renderPointer)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
render | long | 底层视频渲染对象


**返回值**

0/1/2：没有相机权限/打开相机成功/打开相机失败


### 3. 加入会议

**定义**

```
boolean joinRTCByToken(String token,String anyRTCId, String userId, String userData) 
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
token|String|令牌:客户端向自己服务申请获得，参考企业级安全指南
anyRTCId | String | 会议号 (在开发者业务系统中保持唯一的Id)
userId | String | 开发者自己平台的用户Id
userData | String | 开发者自己平台的相关信息（昵称，头像等），可选


**返回值**

true 加入成功 false 加入失败


### 4. 设置显示其他人的视频窗口

**定义**

```
void setRemoteVideoRender(String publishId, long render)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
publishId | String | RTC服务生成的通道Id
render | long |  底层视频渲染对象

**说明**

该方法用于与会者入会成功后，视频即将显示的回调中（onRTCOpenRemoteVideoRender）使用

### 5. 发送消息

**定义**

```
boolean sendMessage(String userName, String headUrl, String content)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userName | String | 用户昵称(最大256字节)，不能为空，否则发送失败；
headUrl | String | 用户头像(最大512字节)，可选；
content | String | 消息内容(最大1024字节)不能为空，否则发送失败；


**返回值**

true 发送成功 false 发送失败

### 6. 设置驾驶模式

**定义**

```
void setDriverMode(boolean enable)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean | bEnable 打开或关闭驾驶模式 true 打开 false 关闭。


### 7. 离开会议

**定义**

```
void leave()
```

### 8. 释放会议对象

**定义**

```
void clean()
```

**说明**  

包含离开会议


### 9. 设置回音消除

**定义**

```
void setForceAecEnable(boolean enable)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean | 是否打开回音消除 true打开 false 关闭 默认关闭


**说明**

必须在joinRTCByToken()之前调用


### 10. 设置本地音频是否传输

**定义**

```
void setLocalAudioEnable(boolean enabled)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean| 打开或关闭本地音频传输

**说明**

true为传输音频，false为不传输音频，默认传输

### 11. 设置本地视频是否传输

**定义**

```
void setLocalVideoEnable(boolean enabled)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean| 打开或关闭本地视频传输

**说明**

true为传输视频，false为不传输视频，默认视频传输

### 12. 获取本地音频传输是否打开

**定义**

```
 boolean getLocalAudioEnabled()
```

**返回值**

音频传输与否

### 13. 获取本地视频传输是否打开

**定义**

```
boolean getLocalVideoEnabled()
```

**返回值**

视频传输与否

### 14. 切换前后摄像头

**定义**

```
void switchCamera()
```

### 15. 设置本地前置摄像头镜像是否打开

**定义**

```
void setFrontCameraMirrorEnable(boolean bEnable)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean | true为打开，alse为关闭 

### 16. 前置摄像头是否镜像

**定义**

```
 boolean getFrontCameraMirror()
```

**返回值**

是否镜像，默认关闭。

### 17. 不接收某人视频

**定义**

```
 void muteRemoteVideoStream(String publishId,boolean mute)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
mute | boolean | true禁止，false接收
publishId | String | RTC服务生成的通道Id 


### 18. 不接收某人音频

**定义**

```
void muteRemoteAudioStream(String publishId,  boolean mute)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
mute | boolean | true禁止，false接收
publishId | String | RTC服务生成的通道Id 

### 19. 设置视频网络状态是否打开

**定义**

```
void setNetworkStatus(boolean enable)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean | true打开，false关闭

**说明**

默认视频网络状态关闭

### 20. 获取当前视频网络状态是否打开

**定义**

```
boolean networkStatusEnabled() 
```

**返回值**

视频网络状态检测打开与否

### 21. 设置音频检测

**定义**

```
void setAudioActiveCheck(boolean open)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
open | boolean | 是否开启音频检测

**说明**

默认音频检测打开

### 22. 获取音频检测是否打开

**定义**

```
boolean isOpenAudioCheck()
```
**返回值**

音频检测打开与否

### 23. 设置视频竖屏

**定义**

```
void setScreenToPortrait()
```

### 24. 设置视频横屏

**定义**

```
void setScreenToLandscape()
```

### 25. 打开共享通道

**定义**

```
void openShare( int type) 
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
type | int | 共享类型，自己平台设定，比如1为白板，2为文档

**说明**  

结果会走onRTCShareEnable()回调


### 26. 设置共享信息

**定义**

```
void setShareInfo(String shareInfo)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
shareInfo | String | 自定义的共享相关信息(限制512字节)

**说明**  

其他人将收到OnRtcUserShareOpen()回调

### 27. 关闭共享通道

**定义**

```
void closeShare( int type) 
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
type | int | 共享类型，canShare方法设定的

**说明**  

其他人将收到onRTCShareClose()回调

### 28. 广播一路视频

**定义**

```
void setBroadCast(String peerId, boolean enable)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String |RTC服务生成的标识身份的ID
enable | boolean |是否广播 setUserToken

**说明**  

**仅用于主持人模式**，主持人设置true（广播）后，其他人将收到此路视频显示的回调（onRTCOpenVideoRender），false 其他人将收到此路视频关闭的回调（onRTCCloseVideoRender）

### 29. 设置单聊

**定义**

```
void setTalkOnly(String peerId,  boolean enable) 
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String |需要单聊的用户的peerId
enable | boolean |是否单聊  true单聊 false结束单聊 

**说明**  

**仅用于主持人模式**，主持人设置后，其他人将听不到主持人的声音，只有单聊的用户能听到

### 30. 设置zoom模式

**定义**

```
void setZoomMode(ARMeetZoomMode mode)
```

**参数**
muteRemoteAudioStream
参数名 | 类型 | 描述
---|:---:|---
mode | ARMeetZoomMode |zoom模式
 

**说明**  

**仅用于Zoom模式**，设置zoom模式

### 31. 设置当前页数

**定义**

```
void setZoomPage( int page)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
page | int |当前页数
 

**说明**  

**仅用于Zoom模式**，设置zoom模式下当前页数

### 32. 设置当前页数及显示个数

**定义**

```
void setZoomPageIdx(int nIdx, int nShowNum)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
nIdx | int |当前页数
 nShowNum | int |需要显示的个数

**说明**  

**仅用于Zoom模式**，设置当前页数及显示个数


---


### ARMeetEvent 回调接口类

### 1. 加入会议成功回调

**定义**

```
void onRTCJoinMeetOK(String meetId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
meetId | String | 会议号 (在joinRTC方法里的第一个参数)

**说明**  

加入会议成功

### 2. 加入会议失败功回调

**定义**

```
void onRTCJoinMeetFailed(String meetId, int code, String reason)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
anyrtcId | String | 会议号 (在joinRTC方法里的第一个参数)
code | int | 错误码 
reason | String | 错误原因

**说明**  

加入会议失败

### 3. 离开会议

**定义**

```
void onRTCLeaveMeet(int code);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int | 状态码 

**说明**  

离开会议状态回调

### 4. 其他与会者加入视频即将显示回调

**定义**

```
void onRTCOpenRemoteVideoRender(String peerId, String publishId, String userId, String userData);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
publishId | String | RTC服务生成的视频通道Id
userId | String | 开发者自己平台的用户Id
userData | String | 开发者自己平台的相关信息（昵称，头像等）

**说明**  

其他与会者进入会议的回调，开发者需调用设置其他与会者视频窗口（setRemoteVideoRender）方法。

### 5. 其他与会者离开

**定义**

```
void onRTCCloseRemoteVideoRender(String peerId, String publishId, String userId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
publishId | String | RTC服务生成的视频通道Id
userId | String | 开发者自己平台的用户Id

**说明**  

其他与会者离开的回调，开发者需移除该与会者视频窗口


### 6. 屏幕共享视频即将显示回调

**定义**

```
void onRTCOpenScreenRender(String peerId, String publishId, String userId, String userData);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
publishId | String | RTC服务生成的屏幕共享视频通道Id
userId | String | 开发者自己平台的用户Id
userData | String | 开发者自己平台的相关信息（昵称，头像等）

**说明**  

屏幕共享视频即将显示回调，开发者需调用设置视频窗口（setRemoteVideoRender）显示屏幕共享流。

### 7. 屏幕共享关闭

**定义**

```
void onRTCCloseVideoRender(String peerId, String publishId, String userId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
publishId | String | RTC服务生成的屏幕共享视频通道Id
userId | String | 开发者自己平台的用户Id

**说明**  

屏幕共享关闭回调，开发者需移除屏幕共享视频窗口

### 8. 音频模式下其他与会者加入

**定义**

```
void onRtcOpenRemoteAudioTrack(String peerId, String userId, String userData);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
userId | String | 开发者自己平台的用户Id
userData | String | 开发者自己平台的相关信息（昵称，头像等）

**说明**  

**音频模式**下其他与会者进入会议的回调。

### 9. 音频模式下其他与会者离开

**定义**

```
void onRtcCloseRemoteAudioTrack(String peerId, String userId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
userId | String | 开发者自己平台的用户Id

**说明**  

**音频模式**下其他与会者离开

### 10. 与会者对音视频的操作通知

**定义**

```
void onRTCRemoteAVStatus(String peerId, boolean audio, boolean video);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
audio | boolean | 音频状态 true 打开 false 关闭
video | boolean | 视频状态 true 打开 false 关闭

**说明**  

与会者关闭/开启了音视频

### 11. 与会者对自己音视频的操作通知

**定义**

```
void onRTCLocalAVStatus(boolean audio, boolean video);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
audio | boolean | 音频状态 true 打开 false 关闭
video | boolean | 视频状态 true 打开 false 关闭

**说明**  

别人对自己音视频的操作

### 12. 音频监测（其他与会者声音大小回调）

**定义**

```
void onRTCRemoteAudioActive(String peerId, String userId, int level, int time);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
userId | String | 开发者自己平台的用户Id
level | int | 音量大小
time | int | time时间内不会再回调该方法(毫秒)

### 13. 音频监测（本地声音大小回调）

**定义**

```
void onRTLocalAudioActive(int level, int time);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
level | int | 音量大小
time | int | time时间内不会再回调该方法(毫秒)

### 14. 本地网络状态

**定义**

```
onRTCLocalNetworkStatus(int netSpeed, int packetLost, ARNetQuality netQuality);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
nNetSpeed | int |网络上行
nPacketLost | int | 丢包率(1~100)
netQuality | ARNetQuality | 网络质量

### 15. 远程（其他人）网络状态

**定义**

```
onRTCRemoteNetworkStatus(String rtcpId, int netSpeed, int packetLost, ARNetQuality netQuality);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId |String | 通道Id
nNetSpeed | int |网络上行
nPacketLost | int | 丢包率(1~100)
netQuality | ARNetQuality | 网络质量

### 16. 网络丢失

**定义**

```
void onRTCConnectionLost()
```

**说明**

会议SDK拥有断网重连机制，断网15秒内恢复可正常开会，15秒没有恢复网络会走该回调，重新恢复网络会自动连接。


### 17. 收到消息

**定义**

```
void onRTCUserMessage(String userId, String userName, String headUrl, String message)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId |String | 发送消息者在自己平台下的Id
userName | String |发送消息者的昵称
headUrl | String | 发送者的头像
message | String | 消息内容

**说明**

收到其他人发送的消息，（该参数来源均为发送消息时所带参数）



### 18. 打开共享通道结果

**定义**

```
void onRTCShareEnable(boolean success)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
success |boolean | true成功 fale失败


**说明**

打开分享功能结果回调


### 19. 收到其他人设置的共享信息

**定义**

```
void onRTCShareOpen(int type, String shareInfo, String userId, String userData);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
type |int | 自定义共享类型
shareInfo | String |自定义共享信息
userId | String | 发起共享者的id
userData | String | 发起共享者的userData


### 20. 分享通道关闭

**定义**

```
void onRTCShareClose();
```


### 21. 主持人上线

**定义**

```
void onRTCHosterOnline(String peerId, String userId, String userData);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId |String |  RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
userId |String |用户在自己平台下的Id
userData | String |用户自定义的userData

**说明**

仅在**主持人模式**下回调

### 22. 主持人下线

**定义**

```
void onRTCHosterOffline(String peerId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId |String |  RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)

**说明**

仅在**主持人模式**下回调


### 23. 用户进入会议室

**定义**

```
void onRtcUserCome(String peerId, String publishId, String userId, String userData);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
publishId | String | RTC服务生成的视频通道Id
userId | String | 开发者自己平台的用户Id
userData | String | 开发者自己平台的相关信息（昵称，头像等）

### 24. 用户离开会议室

**定义**

```
void onRtcUserOut(String peerId, String publishId, String userId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
publishId | String | RTC服务生成的视频通道Id
userId | String | 开发者自己平台的用户Id

### 25. 主持人打开单聊

**定义**

```
void onRTCTalkOnlyOn(String peerId, String userId, String userData);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的标识Id ，用于标识与会者
userId | String | 开发者自己平台的用户Id
userData | String | 开发者自己平台的userData

**说明**

仅在**主持人模式**下回调

### 26. 主持人关闭单聊

**定义**

```
void onRTCTalkOnlyOff(String peerId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的标识Id ，用于标识与会者

**说明**

仅在**主持人模式**下回调

### 27. ZOOM模式下翻页信息

**定义**

```
void onRTCZoomPageInfo(ARMeetZoomMode zoomMode, int allPages, int curPage, int allRender, int screenIndex, int num);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
zoomMode | ARMeetZoomMode |zoom模式
allPages | int |总页数
curPage | int |当前页数
allRender | int |总视频数
screenIndex | int |当前屏幕下标
num | int |一页显示视频个数

**说明**

仅在**ZOOM模式**下回调


## 四、更新日志

**Version 3.0.0 （2019-05-15）**

* SDK版本升级3.0，API接口变更

**Version 2.0.0 （2017-09-30）**

* SDK版本升级2.0，梳理、完善SDK

## 五、错误码对照表

以下为介绍RTCPEngine SDK 的错误码。

名称 | 值            | 备注
---|------------------------------|----
AnyRTC_OK | 0 | 正常
AnyRTC_UNKNOW | 1 | 未知错误
AnyRTC_EXCEPTION | 2 | SDK调用异常
AnyRTC_EXP_UNINIT | 3 | SDK未初始化
AnyRTC_EXP_PARAMS_INVALIDE | 4 | 参数非法
AnyRTC_EXP_NO_NETWORK | 5 | 没有网络链接
AnyRTC_EXP_NOT_FOUND_CAMERA | 6 | 没有找到摄像头设备
AnyRTC_EXP_NO_CAMERA_PERMISSION | 7 | 没有打开摄像头权限
AnyRTC_EXP_NO_AUDIO_PERMISSION | 8 | 没有音频录音权限
AnyRTC_EXP_NOT_SUPPORT_WEBRTC | 9 | 浏览器不支持原生的webrtc
AnyRTC_NET_ERR | 100 | 网络错误 
AnyRTC_NET_DISSCONNECT | 101 | 网络断开
AnyRTC_LIVE_ERR | 102 | 直播出错
AnyRTC_EXP_ERR | 103 | 异常错误
AnyRTC_EXP_Unauthorized | 104 | 服务未授权(仅可能出现在私有云项目)
AnyRTC_BAD_REQ | 201 | 服务不支持的错误请求
AnyRTC_AUTH_FAIL | 202  | 认证失败
AnyRTC_NO_USER | 203 | 此开发者信息不存在
AnyRTC_SVR_ERR | 204 | 服务器内部错误
AnyRTC_SQL_ERR | 205 | 服务器内部数据库错误
AnyRTC_ARREARS | 206 | 账号欠费
AnyRTC_LOCKED | 207 | 账号被锁定
AnyRTC_SERVER_NOT_OPEN | 208 | 服务未开通
AnyRTC_ALLOC_NO_RES | 209 | 没有服务器资源
AnyRTC_SERVER_NOT_SURPPORT | 210 | 不支持的服务
AnyRTC_FORCE_EXIT | 211 | 强制离开

