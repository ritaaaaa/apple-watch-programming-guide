# 配置 Xcode 项目  

Xcode 会将 Watch 应用和 WatchKit 应用扩展一起打包，然后放进现有的 iOS 应用包中。Xcode 提供了一个搭建 Watch 应用的模板，其中包含了创建应用、glance，以及自定义通知界面所需的所有资源。你可以在项目中进行实时测试。

Xcode 中的 WatchKit 应用包已经包含了所有你需要的资源来创建出 WatchKit 应用。包括 glances，自定义通知界面。并且 iOS 模拟器提供了一个实时环境来测试它的界面以及所有的接口。

>
**重要提示**
WatchKit 开发环境需要 iOS 8.2 以上的 SDK，请在 [developer.apple.com](https://developer.apple.com/) 获取最新 SDK。

## 向 iOS 应用中添加 Watch 应用

**要向现有项目中添加 Watch 应用对象，请执行以下操作：**

1. 打开现有的 iOS 应用项目

2. 选择 File > New > Target，然后选中 Apple Watch

3. 选择 Watch App，然后单击 Next

4. 如果您想要使用 glance 或者自定义通知界面，请选择相应的选项我们建议您激活应用通知选项。选中之后就会创建一个新的文件来调试该通知界面。如果您没有选择这个选项，那么之后您只能手动创建这个文件了。

5. 单击 Finish

完成上述操作之后，Xcode 将 WatchKit 应用扩展所需的文件以及 Watch 应用添加到项目当中，并自动配置相应的设置。Xcode 将基于 iOS 应用的 bundle ID 来为两个新对象设置它们的 bundle ID。比如说，iOS 应用的 bundle ID 为 `com.example.MyApp`，那么 Watch 应用的 bundle ID 将被设置为 `com.example.MyApp.watchapp`，WatchKit 应用扩展的bundle ID 被设置为 `com.example.MyApp.watchkitextension`。这三个可执行对象的基本 ID（即 `com.example.MyApp`）必须相匹配，如果您更改了 iOS 应用的 bundle ID，那么您就必须相应的更改另外两个对象的 bundle ID。  

## 应用软件结构

Xcode 中的 WatchKit 应用扩展模板为 iOS 应用创建了两个新的可执行程序。Xcode 同时也配置了项目的编译依赖，从而让 Xcode 在编译 iOS 应用的同时也编译这两个可执行文件。  

图 2-1 说明了它们的依赖关系，并解释了 Xcode 是如何将它们打包在一起的。WatchKit 依赖于 iOS 应用，而其同时又被 Watch 应用依赖。编译 iOS 应用将会将这三个对象同时编译并打包。

**图 2-1** Watch 应用对象的结构

![Watch 应用对象的结构](../images/target_structure_2x.png)  

## 编译、运行以及调试程序

当您创建完 Watch 应用对象后，Xcode 将自行配置用于运行和调试应用的编译方案。使用该配置在 iOS 模拟器或真机上启动并运行您的应用。  

对于包含 glance 或者自定义通知的应用来说，Xcode 会分别为其配置不同的编译方案。使用 glance 配置以在模拟器中调试 glance 界面，使用通知配置以测试静态和动态界面。  

**为 glance 和通知配置自定义编译方案**

1、选择现有的 Watch 应用方案  

2、从方案菜单中选择 Edit Scheme

![image](../images/edit_scheme_menu_2x.png)  

3、复制现有的 Watch 应用方案，然后给新方案取一个合适的名字。

比如说，命名为 “Glance - My Watch app”，表示该方案是专门用来运行和调试 glance。

4、选择方案编辑器左侧栏的Run选项

5、在信息选项卡中，选择合适的可执行对象 

![image](../images/duplicate_scheme_2x.png)  
  
6、关闭方案编辑器以保存更改  

## 指定一个通知进行测试

当您在 iOS 模拟器调试自定义通知界面的时候，您可以指定一个 JSON 负载来模拟推送的通知。通知界面的 Xcode 模板包含一个 RemoteNotificationPayload.json 文件，您可以用它来指定负载中的数据。这个文件位于 WatchKit 应用扩展的 Supporting Files 文件夹。只有当您在创建 Watch 应用时勾选了通知场景选项，这个文件才会被创建。如果这个文件不存在，您可以用一个新的空文件手动创建它。

`RemoteNotificationPayload.json` 文件包含了绝大多数您需要的键值，但是您可以根据应用程序的实际情况添加更多的键值。图 2-2 展示了项目中的默认 JSON 文件。由于 iOS 模拟器无法访问 iOS 应用的注册动作，这个负载便包含了一个名为 “WatchKit Simulator Actions” 的键值，其值为一个包含了一系列动作的数组。每个动作按钮含有 `title` 和 `identifier` 键，它们的值和 iOS 应用中注册的相应键值相同。

**图 2-2** 一个模拟的远程通知负载

![一个模拟的远程通知负载](../images/PushNotificationPayload.apns_2x.png)

绝大部分的 JSON 数据会在运行时打包进字典中进行传输。因为 iOS 模拟器没办法处理 iOS 应用的预置动作。您也许需要负载文件来指定您的动作。WatchKit 模拟器包含了一个行为的数组。数组中的每一个元素包含了以下内容：

* **标题** - 行为的标题，必须包含这个键值
* **id 标示** - 用来传递你的视图控制器的 [application:handleActionWithIdentifier:forLocalNotification:completionHandler:](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intfm/UIApplicationDelegate/application:handleActionWithIdentifier:forLocalNotification:completionHandler:) 或者 [application:handleActionWithIdentifier:forRemoteNotification:completionHandler:](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intfm/UIApplicationDelegate/application:handleActionWithIdentifier:forRemoteNotification:completionHandler:)，必须包含这个键值
* **destructive** - 这个键值包含两个值，`1` 或 `0`，`1` 会导致按钮被渲染成 destructive 的样式。0 会导致按钮被渲染成普通模式，这个键值是可选的。

为了用 JSON 负载测试您的通知接口，您需要用 appropriate 负载文件来配置 build scheme。当您选择了一个通知接口的可执行文件，Xcode 会添加一个菜单来选择您的负载文件。您可以用不同的 build schemes 来加载不同的配置文件，或者您也可以修改以前的负载文件来达到更新的目的。