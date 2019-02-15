# USB

## chrome://inspect

### 使用场景

- 调试真机上的dom
- 调试真机上的js
- 安卓机 + PC + chrome浏览器

### 操作指南

#### 1、手机调试模式连接电脑

	可命令查看是否连接adb devices

#### 2、手机上打开一个网页

#### 3、电脑chrome浏览器地址栏输入
	chrome://inspect/#devices

#### 4、正常会看到下图

![image](https://github.com/winfredwyw/notes/blob/master/assets/201902/DEOKuEuhoe6vAP9YFgCIc8rQ.png)

#### 5、找到你想调试的那个webview，点击inspect，便可出现调试面板


### 常见问题

#### 1、手机连接不上电脑？

	1) 请确保USB连线能正常连通PC和手机（非常重要呀！）

	简单确认方法：手机状态栏显示为充电状态。
	
	2) 请确保PC的USB驱动已正常安装，可以在控制面板“设备管理器” “通用串行总线控制器”中确认无异常USB设备显示。
	
	3) 点击重置，再次“启动检测”。
	
	方案二： 使用adb启动
	
	1) 使用内置adb包（ win下载http://res.imtt.qq.com/TES/winadb.zip, mac下载http://res.imtt.qq.com/TES/macadb.zip），手动启动adb
	
	运行 adb devices
	
	2) 找到设备，点击重置，再次“启动检测”
	
	3) 未找到设备，或者offline
	
	　　① 运行命令 adb kill-server -> adb start-server -> adb devices
	
	　　② 重拔usb链接调试设备
	
	　　③ 重启调试设备
	
	　　④ 重启电脑...
	
	方案三： 安装PC应用宝
	
	方案四： 安装android SDK

#### 2、手机确定连接电脑了，但chrome://inspect/#devices界面为空白或找不到对应的webview
	
	1）目前发现，翻墙后可能会有帮助（要能访问google.com）
	2）webview所在的app换成debug包试试

## Mac Safari Web Inspector

### 使用场景

- 调试真机上的dom
- 调试真机上的js
- iPhone机 + Mac电脑 + safari

### 操作指南

#### 1、启用Mac端safari的开发模式

在Mac下，进到safari->偏好设置->高级，勾选 在菜单栏显示”开发“菜单
![image](https://github.com/winfredwyw/notes/blob/master/assets/201902/UkI6BRqGu9J6Kh_V8T8ljJa5.png)

#### 2、启用ios上safari的Web检查器

在ios下，进到 设置->safari->高级，勾选 Web检查器

![image](https://github.com/winfredwyw/notes/blob/master/assets/201902/612SPiYpwoQgnRqQ8OJ4hH1n.png)

#### 3、 连接Mac电脑与ios设备

将ios设备通过usb接口连接到mac电脑，然后进到 开发 菜单，会显示当前已经连接的ios设备。

在ios的safari打开页面，在Mac端也会显示当前该ios设备可调试的页面，选中页面进入对该页面的调试。
![image](https://github.com/winfredwyw/notes/blob/master/assets/201902/OpewtY3YTTTqav_7N4cvpFN5.png)


#### 4、点击对应的页面，即可出现调试面板