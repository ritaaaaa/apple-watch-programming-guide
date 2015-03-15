# 管理自定义 Long Look 界面

自定义 long-look 通知界面包含两个独立部分，一个是静态的，一个是动态的。静态的界面是必须的，并简单的显示通知的提示信息，它的图片和文字是在设计的时候就配置的。动态界面是可选的，只是为了让你有方法能自定义显示通知内容。

当你在 storyboard 中创建一个新的通知界面 controller 的时候，Xcode 只会默认创建一个静态界面，如果你需要创建动态界面，需要在 storyboard 的通知目录对象中选择 “Has Dynamic Interface” 属性。（只有在静态界面无法满足你需要的时，再选择添加动态界面才是明智的选择）。图 16-1 显示了 storyboard 文件中未修改的静态和动态界面场景。静态和动态场景都是同一类型的相关通知，你可以使用通知 category 对象附加到静态场景来配置。  

**图 16-1** 静态和动态的通知界面   

![静态和动态的通知界面](../images/notification_static_dynamic_2x.png)  

当合法的通知类型被接受后，WatchKit 会基于不同的因素来选择静态或者动态界面。当动态页面不存在，或者当电量不够，又或者你根本就没有明确告诉 WatchKit 显示动态界面的时候，WatchKit 会自动显示静态页面。在其他情况下，WatchKit 会显示你的动态页面。在确定显示动态页面后，WatchKit 会加载合适的 storyboard 资源然后准备合适的接口。就如图 16-2 中所描述。
动态界面的加载过程就跟你应用程序中的界面 controller 一样，只是这个是通知界面 controller 特定的用来处理通知负载数据的。  

**图 16-2** 准备通知界面   

![准备通知界面](../images/notification_process_2x.png)   

## 添加自定义通知界面到你的程序

当你为你的目标应用程序创建 WatchKit 程序时，你需要勾选上 Include Notification Scene 选项来创建对应通知接口的实现文件。Xcode 提供了一个空的 storyboard 场景和一个用于你通知接口 controller 的自定义子类。如果你在创建的时候没有选中刚才说的那个选项，而你又需要创建通知接口，则需要手动自己添加。

手动添加通知接口：拖一个通知接口 controller 对象到你的 storyboard 文件。新的接口仅仅只是包含了静态的页面控制器，为了创建动态接口，你必须执行以下配置步骤：

**配置动态通知接口controller**  

1. 在你的项目中，创建一个新的 [WKUserNotificationInterfaceController](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKUserNotificationInterfaceController_class/index.html#//apple_ref/occ/cl/WKUserNotificationInterfaceController)子类。   
创建新的资源文件并将其添加到你的 WatchKit 拓展目标，然后给你的类一个合适的名字以区别于其他通知接口 controller。

2. 勾选通知目录中得 Has Dynamic Interface，这一步将添加动态场景到你的 storyboard 文件。

3. 设置你动态通知界面 controller 为在第一步创建的类。

应用程序可能有多个通知界面，你可以使用目录（categories）来区分。再你得 storyboard 文件中，使用 Notification Category 对象指定 `category` 名字来关联不同的场景。WatchKit 使用 category 值来决定在运行时加载哪个场景。如果进来的通知没有 category，WatchKit 则加载 category 名为 `default` 的。更多关于如何指定通知的 category，请见 “[Configuring the Category of a Custom Interface](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/CustomzingthePushNotificationInterface.html)”。

## 配置自定义界面的 Category

每个通知接口必须有指定的通知 category，这样才能告诉 Apple Watch 什么时候使用。发送进来的通知可以在负载数据中包含一个 category 键，值则你自己定义。Apple Watch 使用 category 来决定显示哪个通知场景。如果进来的通知没有包含 category 字符串，Apple Watch 显示配置时的默认通知界面。

指定你通知接口的通知类型，在你的 storyboard 中选择 “Notification Category” 对象并选择 Attributes 检查框，如图 16-3 所示。在检查框中得 Name 字段里输入 category 的名字。同时你也能在这个框中设置你自定义界面的 sash 颜色和标题文字。

**图 16-3** 配置通知类型信息   

![配置通知类型信息](../images/notification_type_config_2x.png)    

当生成远程通知时，你的服务器通过在负载数据的 `aps` 字典里包含一个 `category` 键来指定通知类型。对于本地通知，你可以通过 `UILocalNotification` 对象的 [category](https://developer.apple.com/library/prerelease/ios/documentation/iPhone/Reference/UILocalNotification_Class/index.html#//apple_ref/occ/instp/UILocalNotification/category) 属性的值来指定。

>
注意   
category 字符串同样也能定义哪些活动按钮（如果有的话）被追加到通知界面的最后，获取更多关于支持自定义活动的信息，请见 [Adding Action Buttons to Notifications](../notifications/notification-essentials.md)。

## 配置静态通知界面

使用静态通知界面来定义一个简单的自定义的通知界面，静态界面的目的是为了在你 WatchKit 拓展的事件中提供一个回调接口，WatchKit 拓展是无法及时配置动态界面的，以下是创建一个静态界面的规则：

- 所欲的图像必须位于 WatchKit 应用程序包。

- 界面不能包含控件，表格，地图或者其他互动元素。

- 界面的 `notificationAlertLabel` 出口必须连接到一个 label，这个 label 的内容设置为通知的提示信息。对于所有其他标签的文字都不要改变，而要再你的 storyboard 文件中指定。    

图 16-4 展示了在一个日历程序中自定义通知界面的静态和动态场景。通知箭头指向了静态场景，这个场景中包含了一个自定义 icon 和两个label。在静态界面中，label 包含了一个字符串 `<message>`，它就是跟 `notificationAlertLabel` 出口相关的，因此它会在运行时收到通知的提示消息。

**图 16-4** 一个单一通知类型的静态和动态场景    

![一个单一通知类型的静态和动态场景](../iamges//notification_design_2x.png)  

## 配置动态通知界面   

一个动态通知界面可以为用户提供更加丰富的体验。使用动态界面，你可以显示更多而不仅仅只是提示消息。你可以放置更多的信息，配置更多的 label，显示动态生成的内容等等。

实现动态的通知界面需要创建一个 [WKUserNotificationInterfaceController](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKUserNotificationInterfaceController_class/index.html#//apple_ref/occ/cl/WKUserNotificationInterfaceController) 的子类。当一个合适类型的通知过来时，这个子类的实现就是用来响应它的。  

### 设计你的动态界面

配置你得动态界面就像其他界面 controller 场景，包括在你的子类中引用的 label，image 和其他场景中的对象。然后使用这些在运行时配置场景内容。点击通知页面启动应用程序，所以通知界面不应该包含交互控制。

- 多为你的界面使用 label，image，group 和 separator（分离器）。

- 你可以可以在你的界面中包含 table 和 map。

- 不要包含 button，switch 或者其他交互控制。

### 在运行时配置你的动态界面

当一个合适类型的通知到达时，WatchKit 从你的 storyboard 中选择合适的场景显示并告诉你的 WatchKit 拓展初始化对应 [WKUserNotificationInterfaceController](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKUserNotificationInterfaceController_class/index.html#//apple_ref/occ/cl/WKUserNotificationInterfaceController) 子类。图 16-5 展示了 WatchKit 需要准备你界面的步骤，在初始化通知界面 controller 之后，WatchKit 使用 [didReceiveRemoteNotification:withCompletion:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKUserNotificationInterfaceController_class/index.html#//apple_ref/occ/instm/WKUserNotificationInterfaceController/didReceiveRemoteNotification:withCompletion:) 或者 [didReceiveLocalNotification:withCompletion:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKUserNotificationInterfaceController_class/index.html#//apple_ref/occ/instm/WKUserNotificationInterfaceController/didReceiveLocalNotification:withCompletion:) 方法来发送有效负载数据，然后你使用负载数据来配置你通知界面的其他部分，然后调用设置完成处理程序块让 WatchKit 知道你的界面已经准备好了。

**图 16-5** 配置动态通知界面    

![配置动态通知界面](../images//notification_event_cycle_2x.png)  

总是使用 [didReceiveRemoteNotification:withCompletion:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKUserNotificationInterfaceController_class/index.html#//apple_ref/occ/instm/WKUserNotificationInterfaceController/didReceiveRemoteNotification:withCompletion:) 或者 [didReceiveLocalNotification:withCompletion:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKUserNotificationInterfaceController_class/index.html#//apple_ref/occ/instm/WKUserNotificationInterfaceController/didReceiveLocalNotification:withCompletion:) 方法来配置你的通知界面，当实现了其中任意方法后，已经配置的界面提供的处理程序会尽快执行完成。如果等得太久，Apple Watch 会尝试放弃显示你的动态界面而以显示静态界面来代替。

清单 16-1 展示了 `didReceiveRemoteNotification:withCompletion:` 方法实现的示例。这个方法是通过一个虚构的日历程序，发送一个新的会议邀请远程通知。该方法从远程通知的负载数据中提取数据并利用该数据来设置通知界面上的 label 值。为简单起见，该示例假定服务器总是为每个 key 都包含了一个合适的值。但你自己的代码应该做一些必要的错误检查以保证这些负载数据是有效的。在配置完 label 之后，则调用处理程序让 WatchKit 知道自定义的界面以及准备好可以显示了。

清单 16-1 在一个远程通知里配置自定义界面   

```
// Standard remote notification payload keys.
NSString* apsKeyString = @"aps";
NSString* titleKeyString = @"title";

// Payload keys that are specific to the app.
NSString* customDataKey = @"cal";
NSString* invitationDateKey = @"date";
NSString* invitationLocationKey = @"loc";
NSString* invitationNotesKey = @"note";

- (void)didReceiveRemoteNotification:(NSDictionary *)remoteNotification withCompletion:(void(^)(WKUserNotificationInterfaceType interface)) completionHandler {
    // Get the aps dictionary from the payload.
    NSDictionary* apsDict = [remoteNotification objectForKey:apsKeyString];
    
    // Retrieve the title of the invitation.
    NSString* titleString = [apsDict objectForKey:titleKeyString];
    [self.titleLabel setText:titleString];
    
    // Extract the date and time from the custom section of the payload.
    // The date/time information is stored as the number of seconds since 1970.
    NSDictionary* customDataDict = [remoteNotification objectForKey:customDataKey];
    NSNumber* dateValue = [customDataDict objectForKey:invitationDateKey];
    NSDate* inviteDate = [NSDate dateWithTimeIntervalSince1970:[dateValue doubleValue]];

    // Format the date and time strings.
    NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
    
    // Call a custom method to get the localized date format string for the user.
    // The default date format string is "EEE, MMM d".
    dateFormatter.dateFormat = [self dateFormatForCurrentUser];
    NSString *formattedDateString = [dateFormatter stringFromDate:inviteDate];
    
    // Call a custom method to get the localized time format string for the user.
    // The default time format string is "h:mm a".
    dateFormatter.dateFormat = [self timeFormatForCurrentUser];
    NSString *formattedTimeString = [dateFormatter stringFromDate:inviteDate];
    
    // Set the date and time in the corresponding labels.
    [self.dateLabel setText:formattedDateString];
    [self.timeLabel setText:formattedTimeString];
    
    // Set the location of the meeting.
    NSString* locationString = [customDataDict objectForKey:invitationLocationKey];
    [self.locationLabel setText:locationString];
    
    // Set the invitation's notes (if any).
    NSString* notesString = [customDataDict objectForKey:invitationNotesKey];
    [self.notesLabel setText:notesString];
    
    // Tell WatchKit to display the custom interface.
    completionHandler(WKUserNotificationInterfaceTypeCustom);
}

在调用完成处理代码块时，如果你希望 WatchKit 显示静态界面，那就指定 [WKUserNotificationInterfaceTypeDefault](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKUserNotificationInterfaceController_class/index.html#//apple_ref/c/econst/WKUserNotificationInterfaceTypeDefault) 常量。

>
**注意**    
通知界面的文字仅仅只支持系统指定的文字字体，如果你需要显示其他字体，最好是将文字嵌入图片中，然后显示那张图片。

## 测试你自定义界面

当你已经准备好在模拟器上测试你的动态界面时，为你的通知界面创建一个自定义运行计划，如果你还没有这么做的话。当你配置界面时，指定一个你希望发送到你的界面并包含测试数据的 JSON 文件。Xcode 会为指定的数据提供自定义的 JSON 文件。

更多关于设置运行计划并配置你的数据的信息，请见 [The Build, Run, Debug Process](../overview/configuring-your-xcode-project.md)。

