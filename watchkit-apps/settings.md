# 设置　　

参数和设置是变动较少的数值，可以用于配置应用程序的行为或外观。如果 WatchKit 应用程序使用参数进行配置，就可以在工程中添加一个 WatchKit 专用设置包来显示这些设置。此设置包置于 iOS 应用程序中，而设置本身会在用户手机上显示。

WatchKit 设置包与 iOS 设置工作包的工作流程相同。此设置包定义了系统控件和各控件修改的参数名称。用户手机的 Apple Watch 应用程序会获取设置包信息以便显示实际控件。当用户更改控件数值时，系统会更新底层参数值。

如需获取设置包工作的详细信息，请参考 [Preferences and Settings Programming Guide](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)。

## 创建 WatchKit 设置包　
　　　
请按照以下操作向 Xcode 项目添加 WatchKit 设置包：
　　　　
1. 选择 File > New > File。　　
2. 在 Apple Watch 选项中选择 WatchKit 设置包并单击 Next。
3. 使用 `Settings-Watch.bundle` 名称来创建设置包，并添加到 iOS 应用程序中。命名 Settings-Watch.bundle 包是为了与 iOS 应用程序包进行区分。　
　
WatchKit 设置包的初始内容与 iOS 应用程序设置包相同，详见清单 12-1。

清单 12-1 WatchKit 设置包的内容

```
Settings-Watch.bundle/ 
    Root.plist 
    en.lproj/ 
        Root.strings
```

关于如何配置设置包内容，请查看 [Implementing an iOS Settings Bundle](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Conceptual/UserDefaults/Preferences/Preferences.html#//apple_ref/doc/uid/10000059i-CH6)。有关设置包的详细信息，请查看 [Settings Application Schema Reference](https://developer.apple.com/library/prerelease/ios/documentation/PreferenceSettings/Conceptual/SettingsApplicationSchemaReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007071)。

## 允许访问 WatchKit 扩展的参数值

WatchKit 设置必须存储在共享组容器中，iOS 应用程序和 WatchKit 扩展均可对其进行访问。因为 WatchKit 设置包位于 iOS 应用程序,系统会默认把参数值写入iOS应用程序容器。如果想访问 WatchKit 扩展,您必须对项目执行以下配置:

- App Group 同时支持 iOS 应用程序和 WatchKit 扩展。     
选择相同的组标识符。
- 为 `Root.plist` 文件添加 `ApplicationGroupContainerIdentifier` 键。　
当配置 App Group 功能时，把此键所对应索引设置为相同的标示符。   
不需要把此键放入属性列表。    

## 运行状态访问设置　　　　

要想访问存储于一组容器中的参数，请使用 [initWithSuiteName:](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/index.html#//apple_ref/occ/instm/NSUserDefaults/initWithSuiteName:) 方法来创建 `NSUserDefaults` 对象。调用此方法时，请指定组容器标示符所用的字符串。然后使用用户默认对象来访问参数值。下面提供了访问自定义组的示例：

**清单 12-2** 访问一个共享组容器的参数   

```
NSUserDefaults *defaults = [[NSUserDefaults alloc] initWithSuiteName:@"group.example.MyWatchKitApp"]; 
BOOL enabled = [defaults boolForKey:@"enabled_preference"];
```

如何访问参数的值，更多信息请参考 [NSUserDefaults Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/index.html#//apple_ref/doc/uid/TP40003764)。
