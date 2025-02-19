## Flutter 双屏支持插件

该插件支持双屏安卓设备，主副屏使用Flutter进行绘制，使用channel实现双屏间的通信交互。

### 使用方式

在pubspec.yaml文件中进行引用：
```
dependencies:
  flutter_subscreen_plugin: ^1.0.0
```
### 使用方法：

使用flutter进行主副屏的绘制，以及使用封装能力进行主副屏交互通信：

#### 1. 在main入口区分主副屏：
```
void main() {
  var defaultRouteName = window.defaultRouteName;
  if ("subMain" == defaultRouteName) {
    viceScreenMain(); 
  } else {
    defaultMain();
  }
}

//主屏ui
void defaultMain() {
  runApp(MainApp());
}

//副屏ui
void viceScreenMain() {
  runApp(SubApp());
}

```
#### 2. 示例：主屏发送数据给副屏
```
SubScreenPlugin.sendMsgToViceScreen("data", params: {"params": "123"});
```
#### 3. 示例：副屏接收主屏数据
```
SubScreenPlugin.viceStream.listen((event) {
      print(event.arguments.toString());
    });
```

#### 4. 提供方法：获取当前设备环境是否支持双屏
```
SubScreenPlugin.isMultipleScreen((result) {
      print("是否支持双屏：$result");
    });
```

#### 5. 提供方法：判断当前应用是否具备 overlay 窗口权限
```
SubScreenPlugin.checkOverlayPermission((result) {
      print("是否支持 overlay：$result");
    });
```

#### 6. 提供方法：申请 overlay 窗口权限，可将副屏设置为持久窗口
```
SubScreenPlugin.requestOverlayPermission();
```

#### 7. 提供方法：开启，关闭副屏
```
SubScreenPlugin.doubleScreenShow();     //开启
SubScreenPlugin.doubleScreenCancel();   //关闭
```

#### 7. 支付设置初始化完成后直接显示副屏
```
android -> values -> attrs.xml 添加配置

<!-- 是否在初始化时自动显示副屏 -->
<bool name="autoShowSubScreenWhenInit">true</bool> 
```


#### 8. 支持对副屏engine进行三方插件扩展
```
android -> mainActivity -> onCreate 方法添加 

FlutterSubscreenPlugin.tripPlugins = arrayListOf(...具体的三方库名...)
例如： 
FlutterSubscreenPlugin.tripPlugins = arrayListOf(VideoPlayerPlugin())
//VideoPlayerPlugin由video-player三方插件提供
```


> 以上使用方式，完整样例可参照插件中的example
