# 配置 Xcode 项目  

Xcode会将Watch应用和WatchKit应用扩展一起打包，然后放进现有的iOS应用包中。Xcode提供了一个搭建Watch应用的模板，其中包含了创建应用、glance，以及自定义通知界面所需的所有资源。你可以在项目中进行实时测试。

## 向 iOS 应用中添加 Watch 应用

要向现有项目中添加Watch应用对象，请执行以下操作：

1. 打开现有的iOS应用项目

2. 选择 File > New > Target，然后选中Apple Watch

3. 选择 Watch App

4. 单击 Next

5. 如果您想要使用glance或者自定义通知界面，请选择相应的选项我们建议您激活应用通知选项。选中之后就会创建一个新的文件来调试该通知界面。如果您没有选择这个选项，那么之后您只能手动创建这个文件了。

6. 单击 Finish

完成上述操作之后，Xcode将WatchKit应用扩展所需的文件以及Watch应用添加到项目当中，并自动配置相应的设置。Xcode将基于iOS应用的bundle ID来为两个新对象设置它们的bundle ID。比如说，iOS应用的bundle ID为`com.example.MyApp`，那么Watch应用的bundle ID将被设置为`com.example.MyApp.watchapp`，WatchKit应用扩展的bundle ID被设置为`com.example.MyApp.watchkitextension`。这三个可执行对象的基本ID（即`com.example.MyApp`）必须相匹配，如果您更改了iOS应用的bundle ID，那么您就必须相应的更改另外两个对象的bundle ID。  

## 应用软件结构

Xcode中的WatchKit应用扩展模板为iOS应用创建了两个新的可执行程序。Xcode同时也配置了项目的编译依赖，从而让Xcode在编译iOS应用的同时也编译这两个可执行文件。  

图2-1说明了它们的依赖关系，并解释了Xcode是如何将它们打包在一起的。WatchKit依赖于iOS应用，而其同时又被Watch应用依赖。编译iOS应用将会将这三个对象同时编译并打包。

**图 2-1 Watch 应用对象的结构**

![image](../images/target_structure_2x.png)  

## 编译、运行以及调试程序

当您创建完Watch应用对象后，Xcode将自行配置用于运行和调试应用的编译方案。使用该配置在iOS模拟器或真机上启动并运行您的应用。  

对于包含glance或者自定义通知的应用来说，Xcode会分别为其配置不同的编译方案。使用glance配置以在模拟器中调试glance界面，使用通知配置以测试静态和动态界面。  

**为glance和通知配置自定义编译方案**

1、选择现有的Watch应用方案  

2、从方案菜单中选择Edit Scheme

![image](../images/edit_scheme_menu_2x.png)  

3、复制现有的Watch应用方案，然后给新方案取一个合适的名字。

比如说，命名为“Glance - My Watch app”，表示该方案是专门用来运行和调试glance。

4、选择方案编辑器左侧栏的Run选项

5、在信息选项卡中，选择合适的可执行对象 

![image](../images/duplicate_scheme_2x.png)  
  
6、关闭方案编辑器以保存更改  

## 指定一个通知进行测试

当您在iOS模拟器调试自定义通知界面的时候，您可以指定一个JSON负载来模拟推送的通知。通知界面的Xcode模板包含一个RemoteNotificationPayload.json文件，您可以用它来指定负载中的数据。这个文件位于WatchKit应用扩展的Supporting Files文件夹。只有当您在创建Watch应用时勾选了通知场景选项，这个文件才会被创建。如果这个文件不存在，您可以用一个新的空文件手动创建它。

`RemoteNotificationPayload.json`文件包含了绝大多数您需要的键值，但是您可以根据应用程序的实际情况添加更多的键值。图2-2展示了项目中的默认JSON文件。由于iOS模拟器无法访问iOS应用的注册动作，这个负载便包含了一个名为“WatchKit Simulator Actions”的键值，其值为一个包含了一系列动作的数组。每个动作按钮含有`title`和`identifier`键，它们的值和iOS应用中注册的相应键值相同。

**图 2-2 一个模拟的远程通知负载**

![image](../images/PushNotificationPayload.apns_2x.png)
