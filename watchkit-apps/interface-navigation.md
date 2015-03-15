# 界面导航 - Interface Navigation

对于那些不只一屏内容的 WatchKit App 来说，你必须选择一种在界面之间导航的方式。WatchKit App 支持两种完全不一样的导航方式：

* **翻页模式** 这种方式适合那些数据模型较简单的 App ，每页的内容和其它页面没有什么关联。翻页模式界面包含两个或更多的相互独立的视图控制器，但任何时候只有一个是可以被显示的。App 运行时，用户通过在屏幕上左右滑动来切换不同的界面。在屏幕下方有小点来指示用户当前页面所在的位置。
* **层级模式** 这种方式适合数据模型较复杂或是数据结构有一定层级关系的 App。层级模式的一般都是基于一个根视图控制器，在这个控制器里面你可以提供一个按钮，点击之后推出一个新的视图控制器来显示在屏幕上。

尽管你不能在 App 里面混合使用两种导航方式，但你可以在层级模式里面使用模态视图。模态视图是一种打断用户正常浏览流程来要求用户输入或是展示信息的一种方式。在翻页模式和层级模式中都可以使用模态视图。模态视图本身可以包含一个视图控制器或是包含多个基于翻页模式的控制器。

## 实现翻页模式界面

通过在 App 的 Storyboard 里面创建从一个视图控制器到下个视图控制器的 next-page 类型的 segue 来设置使用翻页模式。

**在视图控制器之间创建 next-page segue**

* 在 Storyboard 里面为你的每个页面添加视图控制器。
* 按住 Control 键点击 App 的主视图控制器不放，然后拖动到另一个视图控制器。
* 放开鼠标按键。
* 从 segue 类型面板选择 “next page”类型。
* 用同样的方法在其它界面之间创建 segue。你设置 segue 的顺序决定了界面上的显示顺序。

你在 Storyboard 里面定义的 segue 会在 App 启动的时候加载。你可以通过在程序启动的早期调用 [reloadRootControllersWithNames:contexts:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/reloadRootControllersWithNames:contexts:) 方法来动态设置你要显示的页面集。比如你可以在主视图控制器的[init](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/init)方法里面调用以上方法来强制 WatchKit 加载其它的视图集。

所有翻页模式的视图控制器都是在界面显示之前就被创建并初始化好的，但一次只能显示一个。一般来说，Watchkit 显示初始化序列中的第一个视图控制器。如果要改变初始显示的页面，在 [init](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/init) 或是 [awakeWithContext:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/awakeWithContext:) 方法中调用 [becomeCurrentPage](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/becomeCurrentPage) 方法。

用户从一个界面导航到另一个界面的同时，WatchKit 负责激活、取消激活相应的视图控制器。在切换过程中，当前显示的视图控制器的 [didDeactivate](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/didDeactivate) 方法会被调用，接着会调用下一个将要显示的视图控制器的 [willActivate](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/willActivate) 方法。通过视图控制器的 ```willActivate``` 方法来跟新显示最近的数据改变。

## 实现层级模式界面

在层级模式界面，你通过 segue 告诉 WatchKit 什么时候跳转到新的页面，或是调用当前视图控制器的 [pushControllerWithName:context:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/pushControllerWithName:context:) 方法。在 Storyboard 中你从一个视图控制器上的一个按钮，控件组，或是列表中的一行创建 push segue 到另一个视图控制器。如果你喜欢用编程的方式实现 push，在任意视图控制器的 action 方法中调用 ```pushControllerWithName:context:```

在 push 一个新的视图控制器到屏幕上的时候，建议通过 ```pushControllerWithName:context:``` 方法的 ```context``` 参数传入一个数据对象，通过这个对象可以新的视图控制器在显示之前可以获取一些状态信息，同时通过这个对象告诉新的视图控制器哪些数据需要被显示。

被 push 出来的视图控制器会在屏幕的左上角有一个 “<” 标志来提醒用户可以导航回去。当用户点击屏幕左上角或是在屏幕上左滑，WatchKit 会自动隐藏最上面的视图控制器。你也可以通过调用 [popController](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/popController) 方法来编程实现返回，但你不能使 App 的主视图控制器消失。

## 显示模态视图

模态界面通常是临时打断用户的正常浏览来提示或是显示信息。无论你 App 用的是什么导航方式，都可以在任何视图控制器上显示模态视图。要模态的展示一个视图控制器，可以使用以下方法中的任意一个：

* 在 Storyboard 中创建 modal segue。
* 调用 [presentControllerWithName:context:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/presentControllerWithName:context:) 方法展示一个模态视图。
* 调用 [presentControllerWithNames:contexts:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/presentControllerWithNames:contexts:) 方法展示基于翻页模式布局的两个或两个以上的视图。

创建一个 modal segue，连接到你想要显示的视图控制器。如果需要用 segue 显示多个视图，首先用 next-page segue 把需要模态展示的视图控制器连接在一起，这样你就把这些视图控制器用翻页模式组织在一起了。再用modal segue 连接到控制器组的第一个控制器上。如果 modal segue 连接到中间的某个控制器上，那么这个控制器之前的视图将不会显示。

模态视图的左上角显示了视图控制器的标题。当用户点击该标题时，Watchkit 会让模态界面消失。因此最好设置有关闭模态界面提示意义的标题。例如，当显示信息的时候，可以设置为```Done```或```Close```，如果没有为视图控制器设置标题，WatchKit 默认显示 ```Cancel```