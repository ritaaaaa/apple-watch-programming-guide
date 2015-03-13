##文本与标签

如需在WatchKit应用中显示一段文本，请使用标签对象。标签支持格式化文本，并且可以在运行时用代码改变文本内容。

拖动标签对象到对应的storyboard场景中便可添加标签对象。然后便可设置标签的初始化的文本以及格式。WatchKit同时支持标准字体和自定义字体。例子8-1展示了标准字体风格的使用

#####图8-1 使用标准字体风格的标签

![8-1](../images/text_styles_2x.png)

更多的设置标签的相关信息，请参照[WKInterfaceLabel类参考](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceLabel_class/index.html#//apple_ref/doc/uid/TP40014960)

####使用字体

WatchKit应用，glance界面以及通知界面使用标准字体来显示文本。你应用中的Controller可以使用自定义字体来显示文本。(glance和通知界面无法使用自定义字体)为了让你的应用使用自定义字体，你必须安装相应的字体，如下：

* 把你的自定义字体文件包含到WatchKit应用以及WatchKit扩展Bundle里。
* 给WatchKit应用的Info.plist文件添加UIAppFonts关键字并指向你添加的自定义字体。更多关于UIAppFonts关键字的相关信息，请参照[信息属性列表关键字参考](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009247)

```
注意

为了能在运行时用自定义字体创建相应的字符串，你必须把自定义字体包含到你的WatchKit扩展中。attributed string发送到Apple Watch的时候，相应的字体信息也包含在其中。你WatchKit应用bundle中的自定义字体便可以对字符串进行渲染。

```

使用自定义字体来格式化文本时，需要创建一个包含字体信息的attributed string并把它设给标签,如代码8-1所示.字体名称和大小被编码到attributed string中,然后会被用来更新Apple Watch上的标签.如果定义的字体名称不符合系统字体或者你安装的自定义字体的名称，WatchKit会自动使用系统的字体。

#####代码 8-1 在标签中使用自定义字体

```
//用自定义字体的信息设置attributed string

let menloFont = UIFont(name: "Menlo", size: 12.0)!
var fontAttrs = [NSFontAttributeName : menloFont]
var attrString = NSAttributedString(string: "My Text", attributes: fontAttrs)

//把文本设给标签对象
self.label.setAttributedText(attrString)
```

####管理文件输入

WatchKit为获取用户文本输入提供了一个标准的模式界面。界面允许用户通过语音或者选择标准短语以及表情等来进行输入。你可以通过调用当前处于激活状态的Controller的presentTextInputControllerWithSuggestions:allowedInputMode:completion:方法来显示输入界面.界面显示时，你可以指定输入类型并且可以使用一个block来处理结果。你也可以指定一个或者多个初始化短语用来显示在界面上，如图8-2所示。用户可以从可用短语中选择或者输入其他短语。

#####图8-2 收集用户的文本输入

![figure8-2](../images/text_input_phrases_2x.png)


代码8-2展示了如何配置文本输入Controller并处理结果.在指定初始化短语和输入模式后，Controller以异步的方式加载。当用户选择了某一项或者取消了输入后，你的block会在主线程中执行。使用block来获取用户选择的文本或者表情图片并且刷新你的应用。

#####代码8-2

```
NSArray *initialPhrases = @[@"Ley's do lunch," @"Can we meet tomorrow?", @"When are you free?"];

[self presentTetxInputControllerWithSuggestions:initialPhrases allowedInputMode:WKTextInputModeAllwAnimatedEmoji completion:^(NSArray *results){
    if(results && results.count > 0){
        id aResult = [results objectAtInde:0];
        //使用字符串或者图片
    }else{
        //用户没有选择任何东西
     }
 }];
 
 ```
 
 
####文本多语言化
 
 WatchKit可以使用与iOS应用相同的方法来处理多语言环境。
 
* 在storyboards和xib文件中使用xcode的多语言环境支持。多语言环境让你可以仅使用一个storyboard文件便能支持各种本地化语言。storyboard使用的本地化字符串储存在指定语言的字符串文件中。
 
* 使用[NSLocalizedString](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Miscellaneous/Foundation_Functions/index.html#//apple_ref/c/macro/NSLocalizedString)类的宏定义来动态获取本地化字符串。
* 使用[NSNumberFormatter](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSNumberFormatter_Class/index.html#//apple_ref/occ/cl/NSNumberFormatter)类可以通过用户的时区和场景设置来格式化数值。
* 使用[NSDateFormatter](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSDateFormatter_Class/index.html#//apple_ref/occ/cl/NSDateFormatter)类可以通过用户的时区和场景设置来格式化时间和日期。


对你的应用进行多语言化处理的时候，需要考虑的一个主要问题是安排好你的界面以使标签以及其他显示文本的控件有足够的显示空间。比如，当需要放置三个按钮控件的时候，纵向摆放会比横向摆放更妥当。纵向摆放可以在文本变长的时候提供更多的空间给按钮。

更多关于应用的多语言化，请参照[多语言和本地化指南](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/Introduction/Introduction.html#//apple_ref/doc/uid/10000171i)