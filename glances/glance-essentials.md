# 预览概要

预览可以作为用户查看应用重要信息的辅助方法。并不是所有的应用都需要一个预览。预览可以及时提供一些相关的信息。例如，时钟应用的预览可以给用户展示下一次会议的时间，而航空公司的应用可以给用户展示下次航班的登机信息。图 13-1 展示列表示例应用的预览，它显示了已经完成的项目条数和等待完成的项目条数。  

**图 13-1** 列表示例应用的预览界面  

![列表示例应用的预览界面](../images/glance_lister_2x.png)  

预览可以看作是手表插件应用和手表插件的扩展。你的预览界面存在于手表插件应用的 storyboard 文件中，界面是由自定义的 [KInterfaceController](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/cl/WKInterfaceController) 类来管理。然而，预览界面控制器的唯一任务是设置预览界面的内容。预览不支持交互，且当触摸到预览界面时，会立即启动手表插件应用。

## 预览的生命周期

界面预览控制器和其他界面控制器的生命周期是相同的，除了一开始就初始化展现给用户的界面预览控制器。因为在初始化和展示预览之间只要一小点时间就可以完成，包含检查你的 [willActivate](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/willActivate) 方法确保所展示的信息都是最新的。  

更多关于的界面控制器的生命周期可以查看 [WatchKit Extension Life Cycle](../overview/watchKit-app-architecture.md)。  

## 界面预览指导

Xcode 提供了固定的布局来排版你的预览内容。在选择一个布局来为内容使用之后，用下面的指导来填充内容：

- **设置预览来快速传达信息。**不要显示纯文本框。适当的使用一些图形，颜色和动画来传递信息。
- **聚焦于重要的信息数据。**预览不是手表插件应用的替代品。就像你的手表插件应用是 iOS 应用的缩小版应用一样，预览则是手表插件的缩小版 App。
- **在预览界面里面不包含交互控件。**交互控件包含 buttons, switches, sliders, and menus。
- **在预览界面中需要避免图表和地图。**然而却不是禁止，在狭小的空间里面使得地图和图表似乎没有什么作用。
- **及时查看信息显示。**用所有可用的资源，包含时间和位置来提供信息给用户。考虑到在预览界面初始化的时间和显示给用户的时间 之间会有一些变化，你需要记得更新你的预览。
- **用系统的字体来展示所有的字体。**如果需要使用自定义的字体到你的预览，你需要渲染文本成为图像之后展示给用户。

因为一个应用只有一个预览控制器，所以一个控制器必须能够展示你需要展示的信息。
