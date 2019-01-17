# HTML5 DeviceOrientationEvent & DeviceMotionEvent

标签（空格分隔）： HTML5

---

## DeviceOrientationEvent
提供给网页开发者当设备（指手机，平板等移动设备）在浏览页面时物理旋转的信息

![图例][1]

### 示例

```
window.addEventListener('deviceorientation', function(event) {
  console.log(event.alpha + ' : ' + event.beta + ' : ' + event.gamma);
});
```

### 事件属性

| 属性 | 描述 |
| -- | -- |
| absolute |  用来说明设备是提供的旋转数据是否是绝对定位的布尔值。 |
| alpha | 一个表示设备绕z轴旋转的角度（范围在0-360之间）的数字 |
| beta | 一个表示设备绕x轴旋转（范围在－180到180之间）的数字，从前到后的方向为正方向。 |
| gamma | 一个表示设备绕y轴旋转（范围在－90到90之间）的数字，从左向右为正方向。 |

### 属性说明

- alpha

![此处输入图片的描述][2]

- beta

![此处输入图片的描述][3]

- gamma

![此处输入图片的描述][4]

### 参考
[https://github.com/wagerfield/parallax](https://github.com/wagerfield/parallax)

## DeviceMotionEvent
为web开发者提供了关于设备的位置和方向改变的速度的信息。


### 示例

```
window.addEventListener('devicemotion', function(event) {
  console.log(event.acceleration.x + ' m/s2');
});
```

### 事件属性

| 属性 | 描述 |
| -- | -- |
| acceleration | 提供了设备在X,Y,Z轴方向上加速度的对象。加速度的单位为 m/s2。 |
| accelerationIncludingGravity | 提供了设备在X,Y,Z轴方向上带重力的加速度的对象。加速度的单位为 m/s2 |
| rotationRate | 提供了设备在 alpha，beta， gamma轴方向上旋转的速率的对象。旋转速率的单位为 ?°/s 。 |
| interval | 表示从设备获取数据的频率，单位是毫秒。 |


  
## 兼容性

现代主流浏览器基本兼容

![此处输入图片的描述][5]


  [1]: http://www.zhangyunling.com/study/2017/20170125/axes.png
  [2]: http://www.zhangyunling.com/study/2017/20170125/alpha.png
  [3]: http://www.zhangyunling.com/study/2017/20170125/beta.png
  [4]: http://www.zhangyunling.com/study/2017/20170125/gamma.png
  [5]: https://i.loli.net/2018/09/18/5ba0c867cd1c9.png