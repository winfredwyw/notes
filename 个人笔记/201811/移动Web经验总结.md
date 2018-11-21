### video相关

#### 1. 设置 poster 不生效

- 用 PNG 代替 JPG
- 部分内核默认调用 poster, 但开始加载后会调用第一帧的图片, 所以可能是视频问题

### iOS特有

#### 1. body 上 -webkit-tap-highlight-color 导致点击是整个内容隐藏，只展示 color

- -webkit-tap-highlight-color 只局部使用，并使用透明色