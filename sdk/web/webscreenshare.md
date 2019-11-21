# 屏幕共享
# 屏幕共享
Chrome/firefox 屏幕共享

> 注：在开始屏幕共享前，请确保你已完成[URTC Web端SDK](https://github.com/ucloud/urtc-sdk-web)已经集成接入，详见[接入SDK说明](https://github.com/ucloud/urtc-sdk-web/blob/master/Manual.md) 

## 无插件屏幕共享
在 Chrome/firefox 上屏幕共享时 调用publish方法把video字段设为 false， screen 字段设为 true 即可。

如果你使用的chrome/firefox浏览器版本不满足此要求，请使用[URTC屏幕共享插件](http://urtcsdk.cn-bj.ufileos.com/URTC-screen-extention.zip)在 Chrome 上共享屏幕。

```
client.publish(
  {
     audio: boolean, // 必填，指定是否使用麦克风设备
    video: boolean, // 必填，指定是否使用摄像头设备
    screen: boolean, // 必填，指定是否为桌面共享，注意，video 和 screen 不可同时为 true
  },
  succ => {
    console.log("add screen stream success ", succ);
  },
  err => {
    console.log("add screen stream  failure ", err);
  }
);
```

## 使用屏幕共享插件
chrome 72及72版本以上无需下载插件，72版本以下需要下载[URTC屏幕共享插件](http://urtcsdk.cn-bj.ufileos.com/URTC-screen-extention.zip)

打开你的 Chrome 浏览器，点击屏幕右上方的扩展按钮，选择 更多工具 > 扩展程序， 打开开发者模式 > 加载已解压的扩展程序 > 选择 解压的 URTC屏幕共享插件文件夹，即可完成安装

安装 URTC 提供的 Chrome 屏幕共享插件 ，并获取插件的 extensionId，在publish的时候填入 extensionId。

```
client.publish(
  {
    audio: boolean, // 必填，指定是否使用麦克风设备
    video: boolean, // 必填，指定是否使用摄像头设备
    screen: boolean, // 必填，指定是否为桌面共享，注意，video 和 screen 不可同时为 true
    extensionId?: string // 选填，指定使用的Chrome插件的 extensionId，使用Chrome屏幕共享插件时必填
  },
  succ => {
    console.log("add screen stream success ", succ);
  },
  err => {
    console.log("add screen stream  failure ", err);
  }
);
```

* audio 属性建议设置为 false，避免订阅端收到的两路流中都有音频，导致回声，建议音频只推一路流，防止回音
* 在本地共享的时候，本地流的 Client 不要订阅本地的媒体流，否则会增加时长计费。
* 创建屏幕共享流的时候，video/audio 必须设置为 false。






