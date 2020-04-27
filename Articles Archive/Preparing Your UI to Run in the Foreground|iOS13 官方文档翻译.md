# Preparing Your UI to Run in the Foreground|iOS13 官方文档翻译

文章曾发布于简书：https://www.jianshu.com/p/9b4ffa3ba00e

# 让你的 UI 准备好在前台运行

本文链接：[https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_foreground?language=objc](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_foreground?language=objc)

> 配置您的应用以显示在屏幕上。

## 概述

使用前台过渡( foreground transitions )，让您的应用准备将 UI 显示在显示屏上。 应用程序向前台过渡通常是对用户操作的响应。 例如，当用户点击应用程序的图标时，系统会启动该应用程序并将其带到前台。 使用前台过渡来更新应用的用户界面，获取资源并启动处理用户请求所需的服务。

所有状态转换都会导致 UIKit 将通知发送到适当的委托对象：

- 在 iOS13 及更新的系统上 —— 一个 [UISceneDelegate](https://developer.apple.com/documentation/uikit/uiscenedelegate?language=objc) 对象

- 在 iOS12 及更老的系统上 —— 一个[UIApplicationDelegate](https://developer.apple.com/documentation/uikit/uiapplicationdelegate?language=objc) 对象

您可以同时支持两种类型的委托对象，但是 UIKit 总是在可用时使用 `scene delegate`。 UIKit 仅通知与进入前景的特定场景关联的 `scene delegate`。 有关如何配置场景支持的信息，请参阅[指定应用程序支持的场景 Specifying the Scenes Your App Supports](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/specifying_the_scenes_your_app_supports?language=objc)。

## 进入前台时更新应用程序的数据( Data Model )

在启动时，系统将您的应用程序以非活动状态启动，然后再将其转换为前台。 使用您应用的启动时方法( launch-time methods )来执行当时需要执行的所有工作。 对于处于后台的应用程序，UIKit可通过调用以下方法之一将您的应用程序移至非活动状态：

- 对于**支持场景**( support scenes )的应用程序 —— 适合的场景委托对象的 [`sceneWillEnterForeground：`](https://developer.apple.com/documentation/uikit/uiscenedelegate/3197918-scenewillenterforeground?language=objc)方法。

- 对于所有其他应用程序 —— [`applicationWillEnterForeground：`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623076-applicationwillenterforeground?language=objc) 方法。

从后台过渡到前景时，请使用以上方法从磁盘加载资源并从网络获取数据。

有关如何在启动时准备应用程序的信息，请参阅[响应启动应用程序](https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app?language=objc)。

## 在激活时配置用户界面和初始任务

在显示应用程序的用户界面之前，系统会将您的应用程序立即切换到活动状态( active state )。 激活是配置应用程序的UI和运行时行为的好时机； 特别是：

- 根据需要显示应用程序的窗口。

- 根据需要更改当前可见的视图控制器。

- 更新数据以及视图和控件的状态。

- 显示控件以恢复暂停的游戏。

- 启动或恢复用于执行任务的所有调度队列。

- 更新数据源对象。

- 启动定期任务的计时器。

将您的配置代码放入以下方法之一：

- 对于**基于场景( scene-based )** 的UI —— 适当的场景委托对象的 [`sceneDidBecomeActive：`](https://developer.apple.com/documentation/uikit/uiscenedelegate/3197915-scenedidbecomeactive?language=objc)方法。

- 对于所有其他应用程序 —— 应用程序委托对象的[`applicationDidBecomeActive：`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622956-applicationdidbecomeactive?language=objc)方法。

激活也是在向用户显示用户界面之前对其进行最后润饰的时间。 不要运行任何可能会阻止您的激活方法的代码。 相反，请确保事先拥有所需的一切。 例如，如果您的数据在应用程序外部频繁更改，请在应用程序返回到前台之前，使用后台任务从网络中获取更新。 否则，准备在异步( asynchronously )获取更改时显示现有数据。

## 出现视图时启动只属于 UI 的特定任务

当您的激活方法返回( return )时，UIKit 会显示您显示的所有窗口( shows any windows that you made visible )。 它还通知所有相关的视图控制器( controllers )其视图( views )将要出现。 使用视图控制器的 [`viewWillAppear：`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621510-viewwillappear?language=objc)方法对界面执行所有最终更新。 例如：

- 根据需要启动用户界面动画。

- 如果启用了自动播放，则开始播放媒体文件。

- 开始以全帧速率显示游戏图形和沉浸式内容。

请勿尝试显示其他视图控制器或对用户界面进行重大更改。 当视图控制器出现在屏幕上时，您的界面应该可以显示了( your interface should be ready to display )。
