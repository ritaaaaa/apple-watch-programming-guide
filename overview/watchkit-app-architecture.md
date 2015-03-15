# Watch 应用结构

Apple Watch 应用程序包含两个部分：Watch 应用和 WatchKit 应用扩展。Watch 应用驻留在用户的 Apple Watch 中，只含有故事板和资源文件，要注意它并不包含任何代码。而 WatchKit 应用扩展驻留在用户的 iPhone 上（在关联的 iOS 应用当中），含有相应的代码和管理 Watch 应用界面的资源文件。

当用户开始与 Watch 应用交互时，Apple Watch 将会寻找一个合适的故事板场景来显示。它根据用户是否在查看应用的 glance 界面，是否在查看通知，或者是否在浏览应用的主界面等行为来选择相应的场景。选择完场景后，Watch OS 将通知配对的 iPhone 启动 WatchKit 应用扩展，并加载相应对象的运行界面。所有的消息交流都在后台进行。

**图 3-1** Watch 应用和 WatchKit 应用扩展之间的通信

![Watch 应用和 WatchKit 应用扩展之间的通信](../images/app_communication_2x.png)  

## 场景管理：界面控制器

您 Watch 应用的构建模块是界面控制器，它是 [WKInterfaceController](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/cl/WKInterfaceController) 类的实例。WatchKit 中的界面控制器用来模拟 iOS 中的视图控制器：它显示、管理屏幕上的内容，并且响应用户交互。

如果用户直接启动您的应用，系统将从主故事板文件中加载初始界面控制器。根据用户的交互，您可以显示其他界面控制器以让用户得到需要的信息。如何显示额外的界面控制器取决于您应用所使用的界面样式。WatchKit 支持基于页面的风格以及基于层次的风格。这两个风格不能共存的，要了解更多信息，请参阅：[Interface Navigation](../watchKit-apps/interface-navigation.md)。

>
**重要提示**   
Glances 和通知只会显示一个包含相关信息的界面控制器。用户与界面控制器的交互操作会展示应用的主界面。要了解如何实现 glance，请参阅 [Glance Essentials](../glances/glance-essentials.md)。要了解自定义通知界面，请参阅：[Notification Essentials](../notifications/notification-essentials.md)。

## Watch 应用的生命周期

Apple Watch 上的用户交互将启动您的应用并驱动其生命周期。当用户在 Apple Watch 上运行您的应用时，用户的 iPhone 会自行启动相应的 WatchKit 应用扩展。通过一系列的信号交换，Watch 应用和 Watch 应用扩展将互相连接，因此消息能够在二者之间流通，直到用户停止与应用进行交互为止。此时，iOS 将暂停应用扩展的运行。

随着启动序列的运行，WatchKit 自动为当前界面创建相应的界面控制器。如果用户正在查看 glance，WatchKit 创建出来的界面控制器将与 glance 相连接。如果用户直接启动您的应用，WatchKit 将从应用的主故事板文件中加载初始界面控制器。无论哪种情况，WatchKit 应用扩展都提供一个名为 `WKInterfaceController` 的子类来管理相应的界面。

界面控制器对象初始化后，您就应当为其准备显示相应的界面。图 3-2 展示了 Watch 应用的启动序列。当应用启动时，WatchKit 框架自行创建了相应的 `WKInterfaceController` 对象并调用 initWithContext: 方法。使用该方法来初始化界面控制器，然后加载所需的数据，最后设置所有界面对象的值。对主界面控制器来说，初始化方法紧接着 willActivate 方法运行，以让您知道界面已显示在屏幕上。了解更过，请参阅 [App Essentials](../watchKit-apps/ui-essentials.md)。

**图 3-2** 启动 Watch 应用

![启动 Watch 应用](../images/launch_cycle_2x.png)  

当用户在 Apple Watch 上与应用进行交互时，WatchKit 应用扩展将保持运行。如果用户明确退出应用或者停止与 Apple Watch 进行交互，那么 iOS 将停用当前界面控制器，并暂停应用扩展的运行，如图 3-3 所示。与Apple Watch 的互动是非常短暂的，因此这几个步骤都有可能在数秒之间发生。所以，界面控制器应当尽可能简单，并且不要运行长时任务。重点应当放在读取和显示用户想要的信息上来。

>
**重要提示**   
Glance 界面不支持触摸事件响应。点击你的 glance 界面会直接进入加载应用软件。

您的 Watch Kit 扩展包只有在用户正在触发 Watch 上的事件的时候才会响应。这意味着与您的 Watch 进行数据交互会非常的简短，所以，视图控制器需要尽可能的轻量级，且不能执行耗时的任务。当用户退出了您的应用软件，或者是中断了与 Watch 的交互。iOS 会检测到当前的视图控制器，然后挂起您的操作。

**图 3-3** 界面控制器的生命周期  

![界面控制器的生命周期](../images/watch_app_lifecycle_simple_2x.png)   

## 应用各阶段生命周期中执行的任务

在应用生命周期的不同阶段，iOS 将会调用 WKInterfaceController 对象的相关方法来让您做出相应的操作。表 3-1 列出了大部分您应当在界面控制器中声明的主要方法。


**表 3-1** `WKInterfaceController` 的主要方法   

方法 | 方法的用途
:----------- | :-----------
[init](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/init)         | 初始化你的视图控制器。       
[awakeWithContext:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/awakeWithContext:)         | 这个方法用来准备显示界面。借助它来加载数据，以及更新标签、图像和其他在故事板场景上的界面对象。        
[willActivate](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/willActivate)         | 这个方法可以让您知道该界面是否对用户可视。借助它来更新界面对象，以及完成相应的任务，完成任务只能在界面可视时使用。
[didDeactivate](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/didDeactivate)         | 使用 [didDeactivate](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/didDeactivate) 方法来执行所有的清理任务。例如，使用此方法来废止计时器、停止动画或者停止视频流内容的传输。您不能在这个方法中设置界面控制器对象的值，在本方法被调用之后到 [willActivate](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/willActivate) 方法再次被调用之前，任何更改界面对象的企图都是被忽略的。 


## 在模拟器上进行测试

测试期间，您可以通过锁定与解锁您的模拟器来验证是否达到了你的预期的效果。当您在使用 Hardware > Lock 命令时，WatchKit 会调用当前控制器的 [didDeactivate](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/didDeactivate) 方法。当您解锁了模拟器，WatchKit 会执行当前控制器的 [willActivate](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/willActivate) 方法。  

## 与 Containing iOS 应用共享数据

如果您的 iOS 应用和 WatchKit 应用扩展都依赖于相同的数据，那么您可以使用共享程序组来存储数据。共享程序组是一个位于本地文件系统的区域，应用扩展和应用都能够访问。由于两个程序在不同的沙箱环境中运行，它们一般情况下都不与对方共享文件和进行通信。共享程序组让共享数据成为可能。你可以使用这个空间来存储共享的数据文件或者在两个程序间交换消息。

您可以在 iOS 应用和 WatchKit 应用扩展中的 Capabilities 选项卡中启动共享程序组。激活这项功能后，Xcode 将会为每个对象添加授权文件（需要的话），并给那个文件添加 `com.apple.security.application-groups` 授权。要共享数据，这两个对象必须选择相同的共享程序组。

程序运行时，您可以通过在共享容器目录中读写文件以在两个程序间共享数据。要访问容器，请使用 `NSFileManager` 中的 [containerURLForSecurityApplicationGroupIdentifier:](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSFileManager_Class/index.html#//apple_ref/occ/instm/NSFileManager/containerURLForSecurityApplicationGroupIdentifier:)方法来接收该目录的根路径。使用方法返回的 URL 来枚举目录内容或者在目录中为文件创建新的 URL。

## 与 Containing iOS 应用进行通信

Watch 使用方法 [openParentApplication:reply:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:) 来给 iOS 发送请求并等待回复。WatchKit 扩展包并不支持后台运行；它只会在用户直接主动触碰它时才会响应。containing iOS 应用在设置上有许多限制，他可以被配置成后台收集 WatchKit 扩展包的数据的模式。有些事件可能需要额外的时间来完成，例如接收用户定位的数据，这个就需要通过 iOS 来收集数据，然后传输给 Watch。  

当您使用 `openParentApplication:reply:` 方法后，iOS 会在后台启动或者唤醒 containing iOS 应用，然后调用方法 [application:handleWatchKitExtensionRequest:reply:](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intfm/UIApplicationDelegate/application:handleWatchKitExtensionRequest:reply:) 。这个方法会用提供的字典数据执行请求，然后给 WatchKit 扩展包反馈结果。
