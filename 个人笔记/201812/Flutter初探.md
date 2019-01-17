# Flutter初探

标签（空格分隔）： Flutter

---

## 介绍

Flutter 是 Google 开源的跨平台移动开发框架。它允许从单个代码库为 iOS 和 Android 构建高性能，美观的应用程序。它也是 Google 即将推出的 Fuchsia 操作系统的开发平台。此外，它的架构可以通过定制的 Flutter 引擎将其引入其它平台。


## 开发语言 -- Dart

[为什么Flutter会选择Dart](http://www.infoq.com/cn/articles/why-flutter-uses-dart)

Dart 是一种面向对象的语言，同时支持提前编译和即时编译，非常适合用于构建本地应用程序，同时 Flutter 的热加载有效的提高了开发效率。Dart 提供了垃圾回收，异步，强类型，泛型以及丰富的标准库。Dart 可以编译成 Javascript。 因此 Flutter 可以在网页和移动平台上共享代码。

## start new project

安装 Flutter 后，创建应用非常简单，在终端输入 flutter create [app_name] 命令，或在 VS Code 命令面板中选择“Flutter：New Project”，或在 Android Studio、IntelliJ 中选择“Start a new Flutter Project”。

## IDE

Android Studio 提供了最多的功能，例如 Flutter Inspector 来分析正在运行的应用程序的窗口部件以及监视应用程序性能。 还提供了开发部件层次结构时很方便的几个重构。

VS Code 提供了更轻松的开发体验，因此它的启动速度往往比 Android Studio / IntelliJ 更快。 每个 IDE 都提供内置的编辑助手，如代码补全，接口定义跳转以及良好的调试支持。

## 包管理和插件

## 部件

## 无状态 VS 有状态

## 布局

## 事件

## 绘制

## 动画

## 原生通道

为了在 Android 和 iOS 上提供对本机平台 API 的访问，Flutter 应用程序可以使用平台通道。 这允许 Dart 代码将消息发送到 iOS 或 Android 宿主应用。 许多可用的开源插件都是使用平台通道上的消息传递构建的。要了解如何使用平台通道，Flutter 文档包含一个演示访问本机电池 API 的文档。

## 参考手册

- [中文手册](https://flutterchina.club/docs/)
- [开发者论坛](http://flutter-dev.cn/)
- [Dart手册](https://www.dartlang.org/)

## 辅助文档

- [Flutter的原理及美团的实践](https://mp.weixin.qq.com/s/cJjKZCqc8UuzvEtxK1BJCw)
- [闲鱼技术/Flutter](https://www.yuque.com/xytech/flutter)
- [编写你的第一个Flutter App](https://codelabs.flutter-io.cn/codelabs/first-flutter-app-pt1-cn/index.html#0)
- [关于Flutter，你想知道的都在这里了！](https://mp.weixin.qq.com/s/8OKI-gNu4Qrt298zae43SQ)
- [Flutter原理简解](https://mp.weixin.qq.com/s/CQQXD0TrlbaNWjoClIcDtw)




