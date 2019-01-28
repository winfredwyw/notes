### video相关

#### 1. 设置 poster 不生效

- 用 PNG 代替 JPG
- 部分内核默认调用 poster, 但开始加载后会调用第一帧的图片, 所以可能是视频问题

#### 2. 播放器

- 参考
    - [H5 直播避坑指南](https://zhuanlan.zhihu.com/p/27690199)
    - [X5同层播放器试用报告](https://x5.tencent.com/tbs/guide/web/x5-video.html)

- 微信全屏播放
    - playsinline 
    - webkit-playsinline
    - Android 额外处理
        - x5-video-player-type="h5"
        - x5-video-player-fullscreen="true"
        - x5-video-orientation="portraint" 播放器支付的方向，landscape横屏，portraint竖屏
- 视频内容展示设置
    - object-fit
        - fill  拉伸铺满
        - cover  保持比例铺满，超出隐藏
        - contain  保持比例展示全部内容，不足处留白

- iOS的AirPlay
    -  x-webkit-airplay="allow" 

- 自动播放
    - autoplay
    - 兼容处理
        - Andorid 微信, document.addEventListener("WeixinJSBridgeReady", function (){ 触发播放 })

- 预加载
    - preload="auto"

- 控制栏
    - controls

- 系统播放按钮
    - iOS
        - video::-webkit-media-controls-start-playback-button  display: none;

```
// 微信环境 退出全屏播放时触发
document.getElementById('video').addEventListener("x5videoexitfullscreen", function(){
alert("exit fullscreen")
})
 
// 微信环境 进入全屏播放时触发
document.getElementById('video').addEventListener("x5videoenterfullscreen", function(){
alert("enter fullscreen")
})
```

### iOS特有

#### 1. body 上 -webkit-tap-highlight-color 导致点击是整个内容隐藏，只展示 color

- -webkit-tap-highlight-color 只局部使用，并使用透明色