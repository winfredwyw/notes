## 官方简述

在Web上实现令人惊讶的用户体验的一种新方式。

## 特性

- 可靠 - 即使在不稳定的网络环境下，也能瞬间加载并展现
- 体验 - 快速响应，并且有平滑的动画响应用户的操作
- 粘性 - 像设备上的原生应用，具有沉浸式的用户体验，用户可以添加到桌面

## 相关技术点

PWA是包含了一套技术的解决方案

- App Manifest 的支持度达到 57.43%
- Service Worker 的支持度达到 72.82%
- Notifications API 的支持度达到 43.3%
- Push API 的支持度达到 72.39%
- Background Sync 暂未统计到，Chrome 49 以上均支持

## Service Worker

### 使命

让缓存做到优雅和极致，让 Web App 相对于 Native App 的缺点更加弱化。

### 兼容性

- Chrome 作为开路先锋早早的在 V40 版本就支持了，还提供了完善的 debug 方案
- Firefox，Opera 不甘示弱在后续版本也进行了支持
- 安卓手机 4.x 以上版本新系统形势一片大好（具体各手机的实现还得进一步探测）
- 安卓 Chrome 同样给力
- Apple 方面，从 MacOS Safari 11.1 和 iOS Safari 11.3 开始全面支持
- IE 就不讲了，但是 Edge 从 17 版本开始全面支持

### 前提条件

- 由于 Service Worker 要求 HTTPS 的环境，我们通常可以借助于 github page 进行学习调试。当然一般浏览器允许调试 Service Worker 的时候 host 为 localhost 或者 127.0.0.1 也是 ok 的。
- Service Worker 的缓存机制是依赖 Cache API 实现的
- 依赖 HTML5 fetch API
- 依赖 Promise 实现

### 注册使用

要安装 Service Worker， 我们需要通过在 js 主线程（常规的页面里的 js ）注册 Service Worker 来启动安装。先来感受一段代码：

```
if ('serviceWorker' in navigator) {
    window.addEventListener('load', function () {
        navigator.serviceWorker.register('/sw.js', {scope: '/'})
            .then(function (registration) {

                // 注册成功
                console.log('ServiceWorker registration successful with scope: ', registration.scope);
            })
            .catch(function (err) {

                // 注册失败:(
                console.log('ServiceWorker registration failed: ', err);
            });
    });
}
```
- 这段代码首先是要判断 Service Worker API 的可用情况，支持的话咱们才继续谈实现，否则免谈了。

- 如果支持的话，在页面 onload 的时候注册位于 /sw.js 的 Service Worker。

- 每次页面加载成功后，就会调用 register() 方法，浏览器将会判断 Service Worker 线程是否已注册并做出相应的处理。

- register 方法的 scope 参数是可选的，用于指定你想让 Service Worker 控制的内容的子目录。本 demo 中服务工作线程文件位于根网域， 这意味着服务工作线程的作用域将是整个来源。

- 关于 register 方法的 scope 参数，需要说明一下：Service Worker 线程将接收 scope 指定网域目录上所有事项的 fetch 事件，如果我们的 Service Worker 的 javaScript 文件在 /a/b/sw.js， 不传 scope 值的情况下, scope 的值就是 /a/b。

- scope 的值的意义在于，如果 scope 的值为 /a/b， 那么 Service Worker 线程只能捕获到 path 为 /a/b 开头的( /a/b/page1, /a/b/page2，...)页面的 fetch 事件。通过 scope 的意义我们也能看出 Service Worker 不是服务单个页面的，所以在 Service Worker 的 js 逻辑中全局变量需要慎用。

- then() 函数链式调用我们的 promise，当 promise resolve 的时候，里面的代码就会执行。

- 最后面我们链了一个 catch() 函数，当 promise rejected 才会执行。

- 查看是否注册成功

```
    可以在 PC 上打开我们的好伙伴 chrome 浏览器, 输入 chrome://inspect/#service-workers
```

- 注册失败的原因
```
    为啥会导致 Service Worker 注册失败呢？原因基本就是以下几种情况：

    不是 HTTPS 环境，不是 localhost 或 127.0.0.1。
    Service Worker 文件的地址没有写对，需要相对于 origin。
    Service Worker 文件在不同的 origin 下而不是你的 App 的，这是不被允许的。
```

### 核心功能

#### 缓存

```
// 监听 service worker 的 install 事件
this.addEventListener('install', function (event) {
    // 如果监听到了 service worker 已经安装成功的话，就会调用 event.waitUntil 回调函数
    event.waitUntil(
        // 安装成功后操作 CacheStorage 缓存，使用之前需要先通过 caches.open() 打开对应缓存空间。
        caches.open('my-test-cache-v1').then(function (cache) {
            // 通过 cache 缓存对象的 addAll 方法添加 precache 缓存
            return cache.addAll([
                '/',
                '/index.html',
                '/main.css',
                '/main.js',
                '/image.jpg'
            ]);
        })
    );
});
```

#### 自定义请求响应

```
this.addEventListener('fetch', function (event) {
    event.respondWith(
        caches.match(event.request).then(function (response) {
            // 来来来，代理可以搞一些代理的事情

            // 如果 Service Worker 有自己的返回，就直接返回，减少一次 http 请求
            if (response) {
                return response;
            }

            // 如果 service worker 没有返回，那就得直接请求真实远程服务
            var request = event.request.clone(); // 把原始请求拷过来
            return fetch(request).then(function (httpRes) {

                // http请求的返回已被抓到，可以处置了。

                // 请求失败了，直接返回失败的结果就好了。。
                if (!httpRes || httpRes.status !== 200) {
                    return httpRes;
                }

                // 请求成功的话，将请求缓存起来。
                var responseClone = httpRes.clone();
                caches.open('my-test-cache-v1').then(function (cache) {
                    cache.put(event.request, responseClone);
                });

                return httpRes;
            });
        })
    );
});
```

### 版本更新

#### 自动更新所有页面

```
// 安装阶段跳过等待，直接进入 active
self.addEventListener('install', function (event) {
    event.waitUntil(self.skipWaiting());
});

self.addEventListener('activate', function (event) {
    event.waitUntil(
        Promise.all([

            // 更新客户端
            self.clients.claim(),

            // 清理旧版本
            caches.keys().then(function (cacheList) {
                return Promise.all(
                    cacheList.map(function (cacheName) {
                        if (cacheName !== 'my-test-cache-v1') {
                            return caches.delete(cacheName);
                        }
                    })
                );
            })
        ])
    );
});
```

#### 手动更新 Service Worker

参考如下示例：
```
var version = '1.0.1';

navigator.serviceWorker.register('/sw.js').then(function (reg) {
    if (localStorage.getItem('sw_version') !== version) {
        reg.update().then(function () {
            localStorage.setItem('sw_version', version)
        });
    }
});
```

#### debug 时更新

代码如下：
```
self.addEventListener('install', function () {
    self.skipWaiting();
});
```

### 生命周期

![生命周期](https://gss0.bdstatic.com/9rkZbzqaKgQUohGko9WTAnF6hhy/assets/pwa/projects/1515680651546/sw-lifecycle.png)

- 安装( installing )：这个状态发生在 Service Worker 注册之后，表示开始安装，触发 install 事件回调指定一些静态资源进行离线缓存。
install 事件回调中有两个方法：

- event.waitUntil()：传入一个 Promise 为参数，等到该 Promise 为 resolve 状态为止。

- self.skipWaiting()：self 是当前 context 的 global 变量，执行该方法表示强制当前处在 waiting 状态的 Service Worker 进入 activate 状态。

- 安装后( installed )：Service Worker 已经完成了安装，并且等待其他的 Service Worker 线程被关闭。

- 激活( activating )：在这个状态下没有被其他的 Service Worker 控制的客户端，允许当前的 worker 完成安装，并且清除了其他的 worker 以及关联缓存的旧缓存资源，等待新的 Service Worker 线程被激活。

activate 回调中有两个方法：

- event.waitUntil()：传入一个 Promise 为参数，等到该 Promise 为 resolve 状态为止。

- self.clients.claim()：在 activate 事件回调中执行该方法表示取得页面的控制权, 这样之后打开页面都会使用版本更新的缓存。旧的 Service Worker 脚本不再控制着页面，之后会被停止。

- 激活后( activated )：在这个状态会处理 activate 事件回调 (提供了更新缓存策略的机会)。并可以处理功能性的事件 fetch (请求)、sync (后台同步)、push (推送)。

- 废弃状态 ( redundant )：这个状态表示一个 Service Worker 的生命周期结束。

这里特别说明一下，进入废弃 (redundant) 状态的原因可能为这几种：

- 安装 (install) 失败

- 激活 (activating) 失败

- 新版本的 Service Worker 替换了它并成为激活状态

### 支持的事件

![](https://gss0.bdstatic.com/9rkZbzqaKgQUohGko9WTAnF6hhy/assets/pwa/projects/1515680651547/sw-events.png)

- install：Service Worker 安装成功后被触发的事件，在事件处理函数中可以添加需要缓存的文件（详见 使用 Service Worker ）

- activate：当 Service Worker 安装完成后并进入激活状态，会触发 activate 事件。通过监听 activate 事件你可以做一些预处理，如对旧版本的更新、对无用缓存的清理等。（详见 更新 Service Worker ）

- message：Service Worker 运行于独立 context 中，无法直接访问当前页面主线程的 DOM 等信息，但是通过 postMessage API，可以实现他们之间的消息传递，这样主线程就可以接受 Service Worker 的指令操作 DOM。

Service Worker 有几个重要的功能性的的事件，这些功能性的事件支撑和实现了 Service Worker 的特性。

- fetch (请求)：当浏览器在当前指定的 scope 下发起请求时，会触发 fetch 事件，并得到传有 response 参数的回调函数，回调中就可以做各种代理缓存的事情了。

- push (推送)：push 事件是为推送准备的。不过首先需要了解一下 Notification API 和 PUSH API。通过 PUSH API，当订阅了推送服务后，可以使用推送方式唤醒 Service Worker 以响应来自系统消息传递服务的消息，即使用户已经关闭了页面。

- sync (后台同步)：sync 事件由 background sync (后台同步)发出。background sync 配合 Service Worker 推出的 API，用于为 Service Worker 提供一个可以实现注册和监听同步处理的方法。但它还不在 W3C Web API 标准中。在 Chrome 中这也只是一个实验性功能，需要访问 chrome://flags/#enable-experimental-web-platform-features ，开启该功能，然后重启生效。

### 如何进行 Service Worker 调试

https://lavas.baidu.com/pwa/offline-and-cache-loading/service-worker/service-worker-debug

## PWA兼容性

https://lavas.baidu.com/ready

## 参考

https://lavas.baidu.com/pwa
