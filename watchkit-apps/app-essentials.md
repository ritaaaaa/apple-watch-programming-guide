# App 概要
Storyboard 支持创建 WatchKit App 的界面。当你把 WatchKit App 添加到你的工程后，Xcode会自动生成一个 Storyboard 来让你实现相关UI。这个 Storyboard 至少会有一App个 Scene（场景，一般来说 Storyboard 里面的一个视图控制器就可以看作一个 Scene）是给你的 WatchKit App，也可能还会有预览或通知相关的 Scene。你也可以通过从空间库里面拖拽视图控制器的方式来增加其它 Scene。

在这个 Storyboard 里面每个新建的 Scene 都应该是基于 [WKInterfaceController](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/cl/WKInterfaceController)（对于动态通知界面，则应该继承[WKUserNotificationInterfaceController](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKUserNotificationInterfaceController_class/index.html#//apple_ref/occ/cl/WKUserNotificationInterfaceController)）。对于每个新建的 Scene，记得在id属性框内填上对应的视图控制器的名字。所有和视图控制相关的代码都应该放在对应的 WatchKit 扩展里面。不要在 WatchKit App 里面包含任何代码。

## 视图控制器是如何工作的
视图控制器管理 Scene 里面的内容。视图控制器用 outlet 存储 Scene 里面相应对象的引用，并且通过定义 action 方法来响应用户操作。在 App运行时，通过视图控制器中的 [init](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/init)，[awakeWithContext:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/awakeWithContext:) 和 [willActivate](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/willActivate) 方法来配置界面。在这些方法里，你可以做一些操作，比如：

* 获取需要显示的数据。
* 设置标签、图片或者其它控件的初始值。
* 隐藏那些你不需要一次性同时显示的内容。

WatchKit APP 并不限制有多少视图控制器，但是视图控制器越多也就意味着越复杂，因此少点更好。你选择的导航方式决定了用户是如何从一个界面跳转到下个界面的。有些导航方式需要你明确指明下个视图控制器是哪个，有些则不需要。需要更多关于界面直间导航的内容请参考[这里](./interface-navigation.md)。

## 布局 Scenes
WatchKit App 使用的布局模型不同于 iOS App。在 Scene 里面装配 WatchKit App 界面的时候，并没有因为控件的添加而创建多层次的视图层。而是 Xcode 会决定你所添加到 Scene 的控件元素所在的位置，把他们垂直堆叠在不同的行。在运行时，Apple Watch 会取出相应的控件把它们布局在合适的区域并显示出来。

Watchkit 也提供了让你在 Scene 里面精确放置控件元素的方式。大多数控件的大小和位置可以通过属性面板来设置。另外一种控制控件元素位置的方式是组合对象。组合对象可以让你在水平方向和在竖直方向上一样布局控件。组合对象还可以实现在组合范围内保持控件之间间距。组合对象没有默认的视觉效果，但如果需要的话你也可以配置背景色或是背景图片。

图 5-1 展示了如何在 Storyboard 里面放置不同的控件。前三个控件是标签，每个标签都占满了其所在行的横向区域。对于每个标签，它们的对齐属性决定相对于视图控制器的边界左对齐、右对齐还是居中。标签的下面是由水平方向上排练的两张图片组成的组合对象。在组合对象的下面还有竖直放置的一条分割线和一个按钮。

**图 5-1** Xcode 里的界面对象
![](../images/storyboard_layout_2x.png)

在 Xcode 中创建界面的时候，尽可能让控件自适应大小。App 的界面需要被展示在所有尺寸的 Apple Watch 上。让系统去适配界面，尽量减少为不同设备而写的适配代码。

## 适配不同的显示尺寸

Xcode 支持为不同大小的 Apple Watch 定义不同的界面。在 Storyboard 里面的改动默认会应用到所有不同大小的 Apple Watch，你也可以根据需要为不同的设备定义不同的 Scene。比如，你可能需要为不同的设备调整控件之间的间距和布局以及设置不同的图片。

为不同大小的设备自定义控件需要用到属性面板上的加号按钮来改变默认的设备信息。点击加号按钮为相应的属性增加对应的设备。改变这个值只会影响当前选中的设备。图 5-2 说明了如何为 42mm 的 Apple Watch 设置特殊的字体大小。

**图 5-2** 为不同的设备自定义控件属性
![](../images/scalable_text_2x.png)

不应该让用户发现你的 App 在不同大小的 Apple Watch 上有不同的界面，因此建议尽可能不要为不同的设备定义不同的界面。尽量限制因间距、边距等改变视图布局的做法。不推荐因为不同的布局而移除当前界面上所有控件的做法。你应该尽可能为所有的 Apple Watch 使用相似的界面。

如果要预览不同大小设备上的界面效果，用 Storyboard 编辑区域的控制按钮在不同大小设备间切换。Storyboard 编辑区默认显示任意大小的设备。改变任意大小模式切换到其它所有尺寸的 Apple Watch。如果你讲显示模式切换到了某一个具体的大小，此时你所做的改动只会影响该大小对应的设备。

## 运行时更新界面

运行时，视图控制器可以修改 Scene 中相关对象的以下以下属性：

* 设置或更新数据值。
* 改变控件支持的视觉效果。
* 改变控件的大小。
* 改变控件的透明度。
* 显示、隐藏控件。

不能添加新的控件或是改变已有控件的顺序。尽管你不能移除控件，但是你可以隐藏他们，这样会临时将控件从布局中移除。当所有的控件都隐藏时，其它对象会充满原先这些控件所在的地方。如果要隐藏某个控件而不让其它对象充满该控件所在的位置，你可以把该控件的 alpha 值设置为0。更多关于隐藏控件的内容请参考[这里](./interface-objects.md)。

## 设置 App 的主色




