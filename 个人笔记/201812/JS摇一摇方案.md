# JS摇一摇方案

标签（空格分隔）： 技术方案

---

## 目的

用前端技术，实现Web摇一摇功能

## 摇一摇场景

![此处输入图片的描述][1]
  
## 摇一摇条件分析

- 摇的方向（y轴和x轴的角度变化范围从 -45<sup>o</sup> 到 +45<sup>o</sup>）
- 摇的动作（x,y轴有加速度）

## 加速度变化触发摇一摇事件的阈值

- 假如手机竖着自由落体时，g(x)=g(z)=0,g(y)=9.8
- 摇晃时，假如是45<sup>o</sup>摇晃角度
- x轴和y轴的加速度变化的绝对值，理论上 delta = 9.8 * sin(45<sup>o</sup>) 


## 代码实现

```
// 订阅摇晃事件
var addShakeListener = (function () {
    var _listeners = []; // 订阅者
    var _isListenMotion = false; // 是否已开启监听设备运动

    // 分发回调
    var _dispatch = function () {
        for (var i = 0, j = _listeners.length; i < j; i++) {
            var fun = _listeners[i];

            if (typeof fun === 'function') {
                fun();
            }
        }
    }

    // 监听摇晃事件
    var _listenShake = function () {
        // 记录最小加速度变量
        var minX = 100,
            minY = 100;
        // 触发摇晃加速度变化阈值
        var threshold = 13.8;

        window.addEventListener('devicemotion', function(event) {
            var gravity = event.accelerationIncludingGravity; // 带重力的加速度对象
            var x = gravity.x
                y = gravity.y;
            
            // 记录最小加速度
            if (x < minX) minX = x;
            if (y < minY) minY = y;

            // 与最小加速度，比较加速度变化，是否触发摇晃阈值
            if (Math.abs(x - minX) > threshold && Math.abs(y - minY) > threshold) {
                _dispatch();
                minX = minY = 100;
            }
        });
    }

    return function (fun) {
        _listeners.push(fun);

        !_isListenMotion && _listenShake();
    }
})()

```


## 参考

- [https://juejin.im/post/59741e6f6fb9a06bca0bd7d1#heading-2](https://juejin.im/post/59741e6f6fb9a06bca0bd7d1#heading-2)

[1]: https://github.com/winfredwyw/notes/blob/master/assets/201801/8.webp