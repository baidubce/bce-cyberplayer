# Baidu Cloud Player Web SDK
### 简介
  百度智能云[播放器 Web SDK](https://cloud.baidu.com/doc/Developer/index.html) (以下简称“播放器 SDK”) 是百度官方推出的用于开发网页播放器的软件开发工具包。
### 使用指南

##### 1. 登录百度智能云官网
  使用cyberplayer播放需要先登录[百度智能云官网](https://bce.baidu.com/)。获取AK播放器鉴权做准备。

  -   未注册，须先[注册](UserGuide/注册账号.md#注册百度账号)。
  -   已注册，直接[登录](UserGuide/登录.md)。

##### 2.  获取AK为播放器鉴权做准备

   播放器鉴权将保证您在正式授权的前提下正常使用播放器的各项功能。百度智能云通过验证[AK](Reference/获取AKSK/如何获取AKSK.md) 的方式进行签名授权。若未经授权，播放过程中会显示警告消息。

##### 3.  集成SDK
  
  cyberplayer 的接入步骤包括：引入依赖、添加播放器容器、实例化播放器、使用其他功能。下面为您介绍 cyberplayer 的接入指引。通过接入 cyberplayer，您可以在网页上添加一个视频播放器。
 

###### 1.  引入依赖

    
 cyberplayer 支持以下 2 种资源获取方式。不同获取方式下，引入依赖的操作方法存在差异。
    
 **UMD引入**
 
  请您在本地的项目工程内新建 index.html 文件，在 html 页面内引入 cyberplayer 的脚本文件。
  代码如下所示:
       
       <script src="https://bce-cdn.bj.bcebos.com/jwplayer/4.3.1.2/cyberplayer.js"></script>
    
   **离线包引入**
 
 点击[cyberplayer.zip](https://bce.bdstatic.com/p3m/common-service/uploads/cyberplayer_v4.3.2.1_b0c3dbf.zip)，下载 SDK 压缩包至本地。
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

在您的 Web 应用代码的 html 文件中，通过` <script src="${文件路径}/cyberplayer.js"></script>` 引入依赖。例如，解压在 lib 目录下，引入依赖的代码示例如下所示。

    <script src="./lib/cyberplayer.js" type="text/javascript"></script>

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
                 ak:'XXXXXX', // 公有云平台注册https://cloud.baidu.com/即可获得accessKey,
                 file:"xxx.mp4"
              });

###### 4. 使用其他功能

 支持使用其他功能：
  

*   支持直播（flv、hls、WebRTC）、点播（mp4、flv、hls、ts）播放  
*   支持封面设置、画中画、打点及缩略图、截图等基础功能设置

接入API具体请参考[开发指南](https://cloud.baidu.com/doc/VideoCreatingSDK/s/7ldy776yf)，同时为便于用户便捷开发，百度智能云提供了功能完备的播放器Demo，详见[web播放器演示](http://cyberplayer.bcelive.com/website/index.html)。