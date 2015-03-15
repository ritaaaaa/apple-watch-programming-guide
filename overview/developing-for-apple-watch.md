# 开发您的 Watch 应用 - Developing for Apple Watch

>
**重要提示**  
这是一篇关于使用 API 以及相关技术的初步文档，苹果提供此文档便于你在苹果的相关产品上开发应用软件。后期该文档会有所变动，所以依据本文档开发的应用软件应当使用最新的操作系统进行测试，该文档的新版本会进一步提供 API 以及相关技术支持。  

Apple Watch 给用户呈现了一种人性化，极为容易的获取信息的方式。相较于从口袋里掏出 iPhone 而言，直接看一眼 Apple Watch 来查看信息更加便利。所以，给 Apple Watch 编写的应用程序需要具备极为简易的交互流程，尽可能简单而直白的操作方式。  

![image](../images/watch_hero_2x.png)   

Apple Watch 的运行需要配合 iPhone 运行对应的第三方应用。创建第三方应用需要两个不同的可执行文件：在 Apple Watch 上运行的 Watch 应用，以及在用户 iPhone 上运行的 WatchKit 应用扩展。Watch 应用只包含与应用程序的用户界面相关的 storyboards 以及资源文件。WatchKit 应用扩展则包含了用于管理、监听应用程序的用户界面以及响应用户交互的代码。借助这两个可执行程序，您可以在 Apple Watch 上运行不同类型的用户界面：  

* Watch 应用拥有 iOS 应用的完整用户界面。用户从主界面启动手表应用，来查看或处理信息。
* 使用 glance 界面以便在 Watch 应用上显示即时、相关的信息，该界面是可选的只读界面。并不是所有的 Watch 应用都需要使用 glance 界面。

* 自定义通知界面可以让您修改默认的本地或远程通知界面，并可以添加自定义控件，内容以及设置风格。自定义通知界面是可选的。  

Watch 应用需要尽可能实现 Apple Watch 提供的所有交互动作。由于 Watch 应用目的在于扩展 iOS 应用的功能，因此 Watch 应用和 WatchKit 应用扩展将被捆绑在一起，并且都会被打包进 iOS 应用包当中。如果用户有与 iOS 设备配对了 Apple Watch，那么随着 iOS 应用的安装，系统将会提示用户安装相应的 Watch 应用。  

## Watch 应用

Watch 应用是在 Apple Watch 上交互的主体。Watch 应用通常是从 Apple Watch 的主屏幕上访问，并且能够提供一部分关联 iOS 应用的功能。Watch 应用的目的在于让用户快速浏览相关数据。  

Watch 应用与 iPhone 上运行的 WatchKit 应用（扩展）协同工作，它不包含任何自定义代码，仅仅只是存储了故事板以及和用户界面相关联的资源文件。  

WatchKit 应用扩展是实现这些操作的核心所在。它包含了页面逻辑以及用来管理内容的代码，实现用户操作响应，并且刷新用户界面。由于应用扩展是在用户的 iPhone 上运行，因此它能轻易地和 iOS 应用协同工作，比如说收集坐标位置或者执行其他长期运行任务。  

要开始创建 Watch 应用，请参考：[App Essential](../watchKit-apps/ui-essentials.md) 

## 创建 Glance 界面

Glance 是一个展示即时重要信息的界面。glance 中的内容应当言简意赅，因为其目的是让用户迅速地查看消息。Glance 不支持滚动，因此整个 glance 界面只能在单个界面上显示，您需要保证它拥有合适的大小。glance 只读，因此不能包含按钮、开关，或者其它交互动作。点击 glance 则会直接启动您的 Watch 应用。  

您需要在 WatchKit 应用扩展中添加管理 glance 的代码，用来管理 glance 界面的类与 Watch 应用的类相同。即便如此，创建 glance 更容易实现，因为其无需响应用户交互动作。  

要开始创建 glance 界面，请参阅：[Glance Essentials](../glances/glance-essentials.md)  

## 自定义通知界面

Apple Watch 能够和与之配对的 iPhone 协同工作，来显示本地或者远程通知。Apple Watch 首先使用一个小窗口来显示通知信息。当用户移动手腕希望看到更多的信息时，这个小窗口便会显示更详细的通知内容。应用可以提供详情界面的自定义版本，并且可以添加自定义控件或者改变系统默认的通知信息。  

Apple Watch 支持 iOS 8 中引入的交互式通知。交互式通知通过在通知上添加按钮来让用户立即做出回应。比如说，一个日历时间通知可能会包含了接收或拒绝某个会议邀请的按钮。只要你的 iOS 应用支持交互式通知，那么 Apple Watch 就会自行向自定义或默认通知界面上添加合适的按钮。你所需要做的只是在 WatchKit 应用扩展中处理这些事件而已。  

要开始创建自定义通知界面，请参阅：[Notification Essentials](.../notifications/notification-essentials.md)