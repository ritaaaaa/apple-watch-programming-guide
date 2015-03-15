# 管理你的预览界面 - Managing Your Glance Interface

当你添加手表插件应用到你的 Xcode 项目时，你可以指定是否想要预览界面。如果你忘记在建项目的时候添加预览界面，你也可以晚点再把它加到你的项目中。一个预览界面控制器有轻微的不同显示方式在你的应用的 storyboard 上。尤其是，它有一个预览入口对象附属在上面，同时它有一个默认的布局，如下面的图14-1。

**图 14-1** 一个带有预览入口对象的界面控制器

![一个带有预览入口对象的界面控制器](../images/glance_interface_2x.png)   

你可以用自定义的 [WKInterfaceController](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/cl/WKInterfaceController) 的子类去配置运行时的预览内容。实现子类之后，你可以像实现其它手表插件应用中的界面控制器一样去实现它。  

## 添加预览界面到你的应用

当为你的应用创建手表插件时，为了确保预览屏幕选项包含了创建的相关文件，你需要实现的预览界面。Xcode 提供给你一个预览 storyboard 屏幕和一个自定义的预览界面控制器类。如果你没有在创建的时勾选这个选项，你可以手动去配置。  

**手动创建预览界面**   

1. 在你的项目，创建一个 [WKInterfaceController](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/cl/WKInterfaceController) 的子类。创建这些新的资源文件并添加到你的手表插件扩展目标。给这个子类名一个恰当的名字。例如，`GlanceInterfaceController`。  
2. 在你的 storyboard 文件中，拖拽 Glance Interface Controller 到你的 storyboard。  
这个屏幕展示你的新加入的预览界面控制器就会有一个预览入口对象附属在上面，就如图 14-1 所示。
3. 选择你 storyboard 上的预览界面控制器并打开 Identity inspector。
4. 根据步骤 1 去设置你的预览界面控制器类。  

手表插件应用有且只有一个预览界面。不要添加更多预览界面控制器到你的 storyboard 上。 

## 实现一个预览界面控制器

实现预览界面控制器是相当简单的，因为只需要设置预览容器的标签和图像。所以你的预览通常只需要实现两个步骤。  

- 使用 [init](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/init) 和 [awakeWithContext:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/awakeWithContext:) 方法去初始化你的预览界面并设置它的标签和图像的初始值。
- 根据最近的变化使用 [willActivate](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/willActivate) 去更新预览界面。   

使用一个 [NSTimer](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSTimer_Class/index.html#//apple_ref/occ/cl/NSTimer) 对象周期地变化更新它在屏幕上预览的内容。你无需更新 [WKInterfaceDate](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceDate_class/index.html#//apple_ref/occ/cl/WKInterfaceDate) 和 [WKInterfaceTimer](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceTimer_class/index.html#//apple_ref/occ/cl/WKInterfaceTimer) 对象，因为它们是自动更新的。 

## 定制点击预览之后的启动方式

当用户触摸预览，苹果手表会启动相应的手机插件应用。通常，启动的应用之后会展示它的主界面控制器。如果需要定制点击预览之后的启动方式，按照如下步骤：

- 在预览界面控制器里：
 - 配置预览界的通用方法 [init](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/init) 和 [willActivate](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/willActivate)。   
 - 调用 [updateUserActivity:userInfo:webpageURL:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/updateUserActivity:userInfo:webpageURL:) 方法，并用 userInfo 属性去传递预览的状态信息给你的应用。在启动的时候，你的应用可以用上下文信息来展示不同的界面控制器。
- 在你的主要界面控制器里： 
 - 实现 `handleUserActivity` 方法，用提供的 userInfo 字典去适当配置你的 UI。  
 
在启动的时候，调用 `updateUserActivity:userInfo:webpageURL:` 方法告诉手表插件调用主界面控制器的 [handleUserActivity:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/handleUserActivity:) 方法。用系统提供的的上下文数据适当地配置你的 UI。例如，一个基于页面的界面应用，可以用这些提供的数据信息去选择初始化时展示哪一页。