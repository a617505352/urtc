# 学生端IOS SDK 指南

## 1. 功能说明

  - 支持加入房间
  - 支持房间内发布本地媒体流
  - 支持播放网络音频
  - 支持本地截图
  - 支持本地录制
  - 支持IM自定义消息
  - 支持订阅教师端的MP4流

## 2. 下载资源

  - 可以下载 Demo、SDK、API文档  
    [现在下载URTC](http://urtcsdk.cn-bj.ufileos.com/UCloudRtcSdk_ios.zip)   
    [现在下载IM](http://urtcsdk.cn-bj.ufileos.com/UCloudIMSdk_ios.zip)   


## 3. 开发语言以及系统要求

  - 支持语言：objective-c、swift;  
  - Apple设备：iPhone最低支持iPhone5；  
  - 系统版本：最低支持iOS 9.0；  
  - CPU架构：支持真机架构arm64，不支持模拟器i386、 x86架构；   
  - 其他：不支持bitcode。

## 4. 搭建开发环境  

开发环境：    

  - Xcode 9.0及以上版本；  
  - Apple开发证书或个人账号；  


### 4.1 得到动态库

下载SDK,得到的UCloudRtcSdk\_ios.framework为动态库；  

### 4.2 创建新的工程

使用XCode创建一个新的工程UCloudRtcSdk-ios-demo；  

![创建新的工程.png](/images/sdk/%E5%88%9B%E5%BB%BA%E6%96%B0%E7%9A%84%E5%B7%A5%E7%A8%8B.png)

### 4.3 加入动态库带工程中

将已下载的动态库**UCloudRtcSdk\_ios.framework**加入到**UCloudRtcSdk-ios-demo**工程中**Embedded Binaries**；  

![加入动态库到工程中](/images/sdk/%E5%8A%A0%E5%85%A5%E5%8A%A8%E6%80%81%E5%BA%93%E5%88%B0%E5%B7%A5%E7%A8%8B%E4%B8%AD.png)

### 4.4 打开Xcode

打开Xcode，选择：项目TARGET -\>General-\>Deployment Target，设置8.0或以上版本；  

![设置版本号.png](/images/sdk/%E8%AE%BE%E7%BD%AE%E7%89%88%E6%9C%AC%E5%8F%B7.png) 

### 4.5 使用动态库不需要添加其他库依赖

### 4.6 关闭Bitcode（目前SDK版本不支持Bitcode） 

![关闭bitcode.png](/images/sdk/%E5%85%B3%E9%97%ADbitcode.png) 

### 4.7 编辑info.plist，申请摄像头、麦克风权限

Privacy - Camera Usage Description  
Privacy - Microphone Usage Description  

![编辑info.plist.png](/images/sdk/%E7%BC%96%E8%BE%91info.plist.png) 

### 4.8 打开后台音频权限

为保障APP退入手机后台之后，通话可以保持不中断，建议开启后台音频权限，SDK默认进入后台之后继续推送音频流。

![打开后台音频权限.png](/images/sdk/%E6%89%93%E5%BC%80%E5%90%8E%E5%8F%B0%E9%9F%B3%E9%A2%91%E6%9D%83%E9%99%90.png) 

### 4.9 集成成功

按照上述步骤完成UCloudRtcSdk-ios-demo的前期SDK集成准备之后，请使用Xcode连接iPhone真机，在真机调试环境下，执行编译Commond + B，提示Build Success，表示SDK集成成功。  

## 5. 初始化

建议在初始化 App 的同时，初始化 SDK。  

### 5.1 导入 SDK 头文件  

```objectivec
#import <UCloudRtcSdk_ios/UCloudRtcSdk_ios.h> 
```   

```swift
import UCloudRtcSdk_ios
```

### 5.2 设置 userId 和 roomId，获取AppID;  

```objectivec
UCloudRtcEngine *engine = [[UCloudRtcEngine alloc] initWithUserId:userId  appId:appId roomId:roomId appKey:appKey token:token]];
```

```swift
UCloudRtcEngine *engine = UCloudRtcEngine.init(userId:userId, appId: appId, roomId:roomId , appKey: appKey, token:token)
```

务必要设置代理对象，并实现代理回调方法，设置代理对象失败，会导致 App 收不到相关回调。

```objectivec
engine.delegate = self;
```

```swift
self.engine?.delegate = self
```

### 5.3 配置参数 初始化完成后，即可调用 SDK 相关接口，实现对应功能。 

使用之前需要对SDK进行相关设置，如果不设置，系统将会采用默认值。  

```objectivec
    self.engine.isAutoPublish = YES;//加入房间后将自动发布本地音视频 默认为YES
    self.engine.isAutoSubscribe = YES;//加入房间后将自动订阅远端音视频 默认为YES
    self.engine.isOnlyAudio = NO;//将启用纯音频模式 默认为NO
    self.engine.isDebug = NO;//是否开启日志
    self.engine.videoProfile = UCloudRtcEngine_VideoProfile_360P_1;//设置视频分辨率
    self.engine.streamProfile = UCloudRtcEngine_StreamProfileAll;//设置流权限
```    
```swift
    self.engine?.isAutoPublish = ture;//加入房间后将自动发布本地音视频 默认为ture
    self.engine?.isAutoSubscribe = ture;//加入房间后将自动订阅远端音视频 默认为ture
    self.engine?.isOnlyAudio = false;//将启用纯音频模式 默认为false
    self.engine?.isDebug = false;//是否开启日志
    self.engine?.videoProfile = ._VideoProfile_360P_1;//设置视频分辨率
    self.engine?.streamProfile = .streamProfileAll;//设置流权限
```

## 6. 建立通话

### 6.1 加入房间

```objectivec
[self.engine joinRoomWithcompletionHandler:^(NSData *data, NSUrlResponse *response, NSError error) {
    }];
```  

```swift 
self.engine?.joinRoomWithcompletionHandler({(data, response, error) -> Void in})
```

### 6.2 发布本地流  

1）自动发布模式下，joinRoom成功后，即可发布本地流，无需再次调用publish接口；    
2）手动发布模式下，joinRoom成功后，可通过下述接口发布本地流；

```objectivec
[self.engine publish];
``` 

```swift
self.engine?.publish()
```

3）发布过程中可以监听以下事件获取发布状态，根据状态调用渲染或其他接口即可。    

```objectivec
        - (void)uCloudRtcEngine:(UCloudRtcEngine *)manager didChangePublishState:(UCloudRtcEnginePublishState)publishState {
            switch (publishState) {
                        case UCloudRtcEnginePublishStateUnPublish:
                            self.isConnected = NO;
                        break;
                        case UCloudRtcEnginePublishStatePublishing: {
                            [self.bottomButton setTitle:@"正在发布..." forState:UIControlStateNormal];
                        }
                        break;
                        case UCloudRtcEnginePublishStatePublishSucceed:{
                            self.isConnected = YES;
                            [self.view makeToast:@"发布成功" duration:1.5 position:CSToastPositionCenter];
                            [self.bottomButton setTitle:@"发布成功" forState:UIControlStateNormal];
                        }
                        break;
                        case UCloudRtcEnginePublishStateRepublishing: {
                            [self.bottomButton setTitle:@"正在重新发布..." forState:UIControlStateNormal];
                        }
                        break;
                        case UCloudRtcEnginePublishStatePublishFailed: {
                        self.isConnected = NO;
                            [self.bottomButton setTitle:@"开始发布" forState:UIControlStateNormal];
                        }
                        break;
                        case UCloudRtcEnginePublishStatePublishStoped: {
                        self.isConnected = NO;
                            [self.view makeToast:@"发布已停止" duration:1.5 position:CSToastPositionCenter];
                            [self.bottomButton setTitle:@"开始发布" forState:UIControlStateNormal];
                        }
                        break;
                        default:
                        break;
                    }                               
                }
```

```swift
        func uCloudRtcEngine(_ manager: UCloudRtcEngine, didChange publishState: UCloudRtcEnginePublishState) {
            switch publishState {
                case .unPublish:
                    self.isConnected = false
                case .publishing:
                    CBToast.showToastAction(message: "正在发布...")
                case .publishSucceed:
                    CBToast.showToastAction(message: "发布成功")
                    self.isConnected = true;
                    self.bottomButton?.setTitle("发布成功", for: .normal)
                case .republishing:
                    self.bottomButton?.setTitle("正在重新发布...", for: .normal)
                case .publishFailed:
                    self.isConnected = false;
                    CBToast.showToastAction(message: "开始发布")
                case .publishStoped:
                    self.isConnected = false;
                    CBToast.showToastAction(message: "发布已停止")
                    self.bottomButton?.setTitle("开始发布", for: .normal)
                default:
                break
            }
        }
```


### 6.3 取消发布本地流  

```objectivec
[self.engine unPublish];
```  

```swift
self.engine?.unPublish()
``` 

### 6.4 播放网络音频
---待更新---

### 6.5 本地截图	

```objectivec
    UIGraphicsBeginImageContext(view.frame.size);
    [view drawViewHierarchyInRect:view.bounds afterScreenUpdates:NO];
    UIImage *image =  UIGraphicsGetImageFromCurrentImageContext();
    UIGraphicsEndImageContext();
    UIImageWriteToSavedPhotosAlbum(image, self, @selector(image:didFinishSavingWithError:contextInfo:), NULL);
```

### 6.6 本地录制	
---待更新---

### 6.7 IM自定义消息	
1）导入UCloudIMSdk
```objectivec
#import <UCloudIMSdk/UCloudIMSdk.h>
```
2）初始化UCloudIMEngine
```objectivec
    UCloudIMEngine *imEngine = [UCloudIMEngine sharedImEngine];
    [imEngine cutOffConnect];
    imEngine.delegate = self;
    imEngine.userId = userId;
    imEngine.roomId = roomId;
    imEngine.appId = appId;
```
3）加入房间
```objectivec
    ....
    [self.imEngine joinRoom:parameters completionHandler:^(NSData * _Nullable data, NSError * _Nullable error) {}];
```
4）发送自定义消息
```objectivec
    ....
    [self.imEngine pushCustomContent:parameters completionHandler:^(NSData * _Nullable data, NSError * _Nullable error) {}];
```
### 6.8. 订阅远程流  

1）自动订阅模式下，joinRoom成功后，即可订阅远程流，无需再次调用subscribeMethod接口；    
2）手动订阅模式下，joinRoom成功后，可通过下述接口订阅远程流；   

```objectivec
[self.engine subscribeMethod:remoteStream];
```

```swift
self.engine?.subscribeMethod(remoteStream)
```

3）订阅成功，在回调事件中调用渲染接口即可。  

```objectivec
        -(void)uCloudRtcEngine:(UCloudRtcEngine *)channel didSubscribe:(UCloudRtcStream *)stream{
            [self reloadVideos];
        }
```

```swift
        func uCloudRtcEngine(_ channel: UCloudRtcEngine, didSubscribe stream: UCloudRtcStream) {
            self.reloadVideos()
        }
}
```

### 6.9. 取消订阅远程流

```objectivec
[self.engine unSubscribeMethod:remoteStream];
```

```swift
self.engine?.unSubscribeMethod(remoteStream)
```


### 6.10 离开房间

```objectivec
[self.engine leaveRoom];   
    
```

```swift
self.engine?.leaveRoom()
```

### 6.11 编译、运行，开始体验吧！
