
![](doc/21.gif)
![](doc/17.gif)
![](doc/16.gif)

AiyaCameraSDK 说明文档

[Android版AiyaEffectSDK](https://github.com/aiyaapp/AiyaEffectsAndroid)

[IOS版AiyaEffectsSDK](https://github.com/aiyaapp/AiyaEffectsIOS)

[Android版集成到金山云的示例](https://github.com/aiyaapp/AiyaEffectsWithKSVCAndroid)

[Android版集成到ZegoLive的示例](https://github.com/aiyaapp/AiyaEffectsWithZegoAndroid)

[IOS版集成到金山云的示例](https://github.com/aiyaapp/AiyaEffectsWithKSVCIOS)

[IOS版集成到ZegoLive的示例](https://github.com/aiyaapp/AiyaEffectsWithZegoIOS)

# 1、版本信息
最新版本 V3.0.0

AiyaCamera SDK V3.0.0
>
**功能更新**
- 修复了部分bug
- 资源文件打包
- 人脸识别算法优化
- 加入了更多特效

[历史版本信息](doc/versionHistory.md)

# 2、运行环境说明
AiyaCameraSDK 最低运行版本为iOS8.0

# 3、SDK 功能说明
AiyaEffectsSDK可用于相机、图片处理、直播等多种情景，主要功能如下：

- 2D序列帧特效
- 3D静态特效和动画特效
- 多种美颜算法

# 4、SDK API说明
###### AiyaCamera: 相机组件.
    通过设置启动相机时的参数以及设置美颜和滤镜参数,打开相机,对相机输出的数据进行美颜和滤镜处理.使用前要验证License.

###### AiyaEffectProcess: 图像数据处理组件(特效,美颜).
    设置美颜和滤镜参数,对图像数据进行美颜和滤镜处理.使用前要验证License.

###### AiyaGPUImagexxxFilter: 滤镜.
    特效滤镜,美颜滤镜.

###### AYxxx: GPUImageSDK中的核心类.
    都以AY为前缀.逻辑无修改.

###### AiyaLicenseManager: 验证License.
    在启动时调用.

###### AiyaCameraEffect:
    对视频图像数据进行美颜和特效处理.

# 5、集成说明
## 1. 导入SDK静态库文件AiyaCameraSDK.framework和资源文件 AYEffectTrackerData目录下所有文件和thirdLib目录下的第三方库

## 2. 导入依赖的系统库动态库 libz.tbd, libc++.tbd, libiconv.tbd

## 3. 初始化Lisence.在使用AiyaCameraSDK之前,必须先初始化license,否则会出现无法使用的情况.License申请请访问:http://bbtexiao.aiyaapp.com/site/free 我们在收到您的申请后会及时审批,并把审批结果发送到您的邮箱,请注意查收.
```objective-c
[AiyaLicenseManager initLicense:@"对应的licenseKey"];

```

## 4. 具体使用方式如下
 * 使用方式一: 用AiyaCamera对相机数据进行特效加美颜处理
```objective-c
_camera = [[AiyaCamera alloc]initWithPreview:self.view cameraPosition:AVCaptureDevicePositionFront];//设置为前置相机
[self.camera setSessionPreset:AVCaptureSessionPreset1280x720];
[self.camera setEffectPath:path];//设置特效路径
[self.camera setEffectPlayCount:0];//设置特效播放次数.0表示一直重复
[self.camera setBeautyLevel:AIYA_BEAUTY_LEVEL_5];//设置美颜等级
[self.camera setBeautyType:AIYA_BEAUTY_TYPE_0];//设置美颜类型
[self.camera setHasAudioTrack:YES];//打开音频数据采集
self.camera.delegate = self;
[self.camera startCapture];//打开相机

#pragma mark AiyaCameraDelegate
/**
 特效渲染完成后的数据回调
 */
- (void)videoCaptureOutput:(AiyaCamera *)capture pixelBuffer:(CVPixelBufferRef)pixelBuffer frameTime:(CMTime)frameTime effectStatus:(AIYA_CAMERA_EFFECT_STATUS)effectStatus{

}

/**
 音频数据的回调
 */
- (void)audioCaptureOutput:(AiyaCamera *)capture audioBuffer:(CMSampleBufferRef)audioBuffer{

}
```
 * 使用方式二: 用AiyaEffectProcess对像素缓冲区数据进行处理
```objective-c
_aiyaEffectProcess = [[AiyaEffectProcess alloc]init];//只初始化一次
_aiyaEffectProcess.beautyLevel = AIYA_BEAUTY_LEVEL_5;
_aiyaEffectProcess.beautyType = AIYA_BEAUTY_TYPE_0;
_aiyaEffectProcess.effectPath = path;//设置特效路径
_aiyaEffectProcess.effectPlayCount = 1;//特效只播放一次

//---对像素缓冲区数据进行特效和美颜处理---
[_aiyaEffectProcess processWithPixelBuffer:pixelBuffer];
//---特效和美颜处理结束---
```

# 6、资源说明
贴纸资源制作规范请参照其他相关文档。

# 7、常见问题
问题一: 如果项目中已经集成了GPUImageSDK怎么办?会不会产生冲突
答: AiyaCameraSDK中封装的GPUImageSDK,全部类,c函数,宏都做了加前缀的处理.集成到项目中时不会和原有的GPUImageSDK产生冲突.

问题二: SDKDemo中有Podfile文件,但是我的电脑没有装pod怎么办?
答: SDKDemo已经把第三方库打包进去了.不需要下载更新第三方库.双击AiyaCameraSDKDemo.xcworkspace文件,打开后即可运行SDKDemo.

问题三: 模拟器上无法运行.一直报错找不到文件怎么办?
答: 目前SDK没有打包x86版本,无法在模拟器上运行.只能在真机上运行.

问题四: 没有网络时License会失效吗?
答: 没有网络也可以正常使用.

# 8、License说明
1. 每次启动时都要进行验证.
2. 首次验证会进行联网.
3. License 验证是异步请求,不影响主线程
4. AiyaLicenseManager的initLicense函数返回码说明:
 YES 认证成功, SDK可以正常使用
 NO  认证失败, SDK不可以使用,请通过下面的联系方式联系我们
5. license申请请访问:http://bbtexiao.aiyaapp.com/site/free 我们在收到您的申请后会及时审批,并把审批结果发送到您的邮箱,请注意查收.

# 9、联系方式
邮箱: liudawei@aiyaapp.com
