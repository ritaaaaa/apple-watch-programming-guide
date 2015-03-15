# 使用 iOS 技术

WatchKit 应用扩展可以使用绝大多数 iOS 技术。由于应用扩展和应用还是有一些不同，因此某些技术的使用可能会受到限制，或者不受到推荐。下面是决定是否使用特定技术的准则：

* **避免使用需要用户权限的技术，比如 Core Location。**在 WatchKit 应用扩展中使用该技术的话，可能会在您第一次请求使用时导致用户的 iPhone 上显示出一个意想不到的提示。更糟糕的是，这个状况可能会在任何时间出现，即使用户的 iPhone 在他们的口袋里并且屏幕是锁定的。

* **不要使用后台执行模式的技术。** WatchKit 应用扩展只考虑在前台运行的情况，并且其只在用户和对应的 Watch 应用交互时运行。因此，支持后台运行的某些技术在 WatchKit 应用扩展中并不被支持。

* **避免使用需要长时间运行的技术。** WatchKit 应用扩展在用户停止和对应的 Watch 应用交互后便迅速暂停。由于与 Watch 应用的交互通常是短暂的，因此应用扩展可能会在请求的数据到达前暂停运行。

要使用 iOS 技术，其中一种解决方法是让您的 iOS 应用来使用这些技术。例如，在您的 iOS 应用中使用位置服务，而不是在 WatchKit 中使用这个技术。iOS 应用可以收集所需的数据然后通过共享程序组来让应用扩展稍后访问。

## 支持 Handoff

Watch 支持创建的 Handoff 功能。你可以用控制器 `WKInterfaceController` 中的 [updateUserActivity:userInfo:webpageURL:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/updateUserActivity:userInfo:webpageURL:) 方法来创建。
在 glance 的控制器中，你可以定制有用的信息，来与你的设备进行交互。当用户点击了 glance 上的应用的时候，WatchKit 将会将数据传输到你的设备上。之后，你可以用这个发送过来的数据更新您的设备上的相关内容。

## 远程控制事件与 Now Playing 信息

Watch 可以用远程控制事件来管你与之配对的 iPhone，例如播放音乐、视频。设备也会将当前正在播放的音乐（或者视频）信息传输到 Watch 上显示。一个注册过 [MPRemoteCommandCenter](https://developer.apple.com/library/prerelease/ios/documentation/MediaPlayer/Reference/MPRemoteCommandCenter_Ref/index.html#//apple_ref/occ/cl/MPRemoteCommandCenter) 的iOS应用程序会自动的接受数据处理信息。你不需要在你的WatchKit扩展包中做些额外的操作来同步数据。

>
**重要提示**  
为了反馈类似于喜欢、不喜欢或者标记一个内容的信息，Watch 是使用 [localizedShortTitle](https://developer.apple.com/library/prerelease/ios/documentation/MediaPlayer/Reference/MPFeedbackCommand/index.html#//apple_ref/occ/instp/MPFeedbackCommand/localizedShortTitle)，而不是 `MPFeedbackCommand` 中的 `localizedTitle`。

Now Playing 的 glance 会自动的显示当前 iOS 应用的 Now Playing 信息。iOS 应用程序通过 [MPNowPlayingInfoCenter](https://developer.apple.com/library/prerelease/ios/documentation/MediaPlayer/Reference/MPNowPlayingInfoCenter_Class/index.html#//apple_ref/occ/cl/MPNowPlayingInfoCenter) 来提供这些数据。当您的app在更新数据时，您应当去更新 [nowPlayingInfo](https://developer.apple.com/library/prerelease/ios/documentation/MediaPlayer/Reference/MPNowPlayingInfoCenter_Class/index.html#//apple_ref/occ/instp/MPNowPlayingInfoCenter/nowPlayingInfo) 字典的内容。Watch 会自动的更新数据并显示出来。另外，如果此时用户点击了 Now Playing 上的标题，Watch 会直接进入 iOS 应用对应的 Watch 应用中。

更多的关于如何实现远程控制以及Now Playing的内容，请查阅 [Remote Control Events](https://developer.apple.com/library/prerelease/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Remote-ControlEvents/Remote-ControlEvents.html#//apple_ref/doc/uid/TP40009541-CH7)。