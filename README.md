# Baidu Cloud Player Web SDK
### 简介
  百度智能云[播放器 Web SDK](https://cloud.baidu.com/doc/Developer/index.html) (以下简称“播放器 SDK”) 是百度官方推出的用于开发网页播放器的软件开发工具包。

  百度智能云播放器 SDK Web端（cyberplayer）自 4.4.0.1 版本起需获取 License 授权后方可使用。若您无需使用高级功能，可直接申请标准版 License 继续免费使用；若您需要使用高级功能则需购买高级版
### 使用指南

本章以播放器 Web SDK v4.4.0.1版本为例，适用于源视频文件存于[BOS](https://cloudtest.baidu.com/product/bos.html)、[VOD](https://cloudtest.baidu.com/product/vod.html)及HLS/FLV/WebRTC直播等场景，对于HLS/FLV/WebRTC直播，请您在初始化播放器（即调用setup方法）时，添加配置参数'isLive: true'。

##### 1.准备工作
播放器 SDK Web端（cyberplayer）自 4.4.0.1 版本起需获取 License 授权后方可使用。若您无需使用新增的高级功能，可直接申请标准版 License 以继续免费使用 cyberplayer；若您需要使用新增的高级功能则需购买高级版 cyberplayer。具体信息如下：


| cyberplayer功能 | 功能范围 | 所需 License  | 定价 |授权形态| 授权单位 |
| --- | --- | --- | --- | --- | --- |
| 标准版功能 |标准版功能，详见 [产品功能支持](https://cloud.baidu.com/doc/VideoCreatingSDK/s/9ldy6yw5s#功能支持) | 播放器 Web 端标准版 License | 0元 [免费申请](https://console.bce.baidu.com/bvc/#/bvc/player-license/list) | 有限期授权 |1个license最多授权1个泛域名|
| 高级版功能 | 标准版功能、H265软硬解播放、Av1软硬解播放、MPEG-TS播放| 播放器 Web 端高级版 License**（试用期30天）** |3999元/个/年   [立即购买](https://console.bce.baidu.com/bvc/#/bvc/player-license/list) |有限期授权 |1个license最多授权1个泛域名|
| 高级版功能-永久 |标准版功能、H265软硬解播放、Av1软硬解播放、MPEG-TS播放  | 播放器 Web 端高级版 License**（试用期30天）** | 40000元/个  [立即购买](https://console.bce.baidu.com/bvc/#/bvc/player-license/list)| 无限期授权 |1个license最多授权1个泛域名|


**说明**

*  播放器 Web 端标准版 License 可免费申请,申请后有效期默认1年；到期后可免费申请续期
*  播放器 Web 端高级版 License ,申请后试用期30天；到期后可申请购买
*  为方便本地开发，播放器不会校验 localhost 或者 127.0.01，因此申请 License 时不需要申请这类本地服务域名
*  为方便客户使用，播放器会在License过期7天内提示客户去申请续期，超过7天播放器将会终止使用

##### 2.  集成SDK
  
  cyberplayer 的接入步骤包括：引入依赖、添加播放器容器、实例化播放器、使用其他功能。下面为您介绍 cyberplayer 的接入指引。通过接入 cyberplayer，您可以在网页上添加一个视频播放器。
 

###### 1.  引入依赖

    
 cyberplayer 支持以下 3 种资源获取方式。不同获取方式下，引入依赖的操作方法存在差异。
    
 **UMD引入**
 
  请您在本地的项目工程内新建 index.html 文件，在 html 页面内引入 cyberplayer 的脚本文件。
  代码如下所示:
       
       <script src="https://bce-cdn.bj.bcebos.com/jwplayer/4.4.0.1/cyberplayer.js"></script>
    
   **离线包引入**
 
 点击[cyberplayer.zip](https://bce.bdstatic.com/p3m/common-service/uploads/cyberplayer_v4.4.0.1_581a502.zip)，下载 SDK 压缩包至本地。
将 SDK 压缩包解压到项目文件目录下。例如，解压到 lib 目录下。

解压后的目录结构如下所示:

      cyberplayer-<version>.zip
            │   ├── skins
            │   │   ├── {skin}.css (播放器样式)
            │   ├── cyberplayer.js
            │   ├── missile.js (用于H.265 、AV1软解使用，无需在html中单独引入)
            │   ├── missile.wasm 
            │   ├── missile-simd.js (用于H.265 、AV1软解使用，无需在html中单独引入)
            │   ├── missile-simd.wasm
            │   ├── missile-min.js (用于H.265 、AV1软解使用，无需在html中单独引入)
            │   ├── missile-min.wasm
            │   ├── license.js (用于License鉴权，无需在html中单独引入)
            │   ├── license.wasm

在您的 Web 应用代码的 html 文件中，通过` <script src="${文件路径}/cyberplayer.js"></script>` 引入依赖。例如，解压在 lib 目录下，引入依赖的代码示例如下所示。

    <script src="./lib/cyberplayer.js" type="text/javascript"></script>
    
  **NPM引入**
  
通过包管理工具将 SDK 的依赖安装到项目中。

`npm install @baiduplayer/cyberplayer`

在项目中引入 cyberplayer

`import cyberplayer from '@baiduplayer/cyberplayer';`

###### 2. 添加播放器容器

在需要展示播放器的页面添加播放器容器，例如，在 index.html 中加入以下代码。

    <div id="playerContainer"></div>

###### 3. 实例化播放器

根据业务使用在setup方法传入相关参数使用，代码示例如下所示。

     const playerSdk = cyberplayer('playerContainer').setup({
                 width:640,
                 height:360,
                 autostart:true,
                 stretching:'uniform',
                 volume:100,
                 controls:true,
                 appid:'XXXXp',  // 参考准备工作部分，appid对应百度智能云控制台申请 License 后的licenseID
                 licenseUrl:'./lib/XXX.license',  // License文件路径，百度智能云控制台申请后，将下载下来的.license文件存放到项目目录中，以静态资源方式传入。注意：.license文件名不能更改
                 file:"xxx.mp4"
              });

###### 4. 使用其他功能

 支持使用其他功能：
  

*   支持直播（flv、hls、WebRTC）、点播（mp4、flv、hls、ts）播放  
*   支持封面设置、画中画、打点及缩略图、截图等基础功能设置

接入API具体请参考[开发指南](https://cloud.baidu.com/doc/VideoCreatingSDK/s/7ldy776yf)，同时为便于用户便捷开发，百度智能云提供了功能完备的播放器Demo，详见[web播放器演示](http://cyberplayer.bcelive.com/website/index.html)。


  
  
