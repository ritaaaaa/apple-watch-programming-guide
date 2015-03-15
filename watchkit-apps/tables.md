# Tables

使用 tables 来显示整列动态改变的数据。WatchKit 支持单列Tables(使用[WKInterfaceTable类](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceTable_class/index.html#//apple_ref/occ/cl/WKInterfaceTable))。以表格的形式显示数据要求预先定义显示数据的布局并且在运行时把数据填充到表格中。具体来说，你需要在你的项目中做以下事情：

- 在storyboard文件中：
 - 添加table对象到你的Controller场景中。给table对象创建一个输出口（outlet)
 - 依照 [配置你的行类型](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Tables.html#//apple_ref/doc/uid/TP40014969-CH14-SW6)中描述的方式来配置你表格中的行控制器.
- 在代码中:
 - 定义一个行控制器类用以操作创建的表格的每一行(见 [配置行类型](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Tables.html#//apple_ref/doc/uid/TP40014969-CH14-SW6))
 - 在初始化的时候添加行到表格(见 [运行时配置表格内容](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Tables.html#//apple_ref/doc/uid/TP40014969-CH14-SW4))
 - 实现事件输入反馈(见 [处理行选择](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Tables.html#//apple_ref/doc/uid/TP40014969-CH14-SW5))
   
你可以给表格定义多种类型的行控制器，每一个都可以有不同的外观。你可以在运行时确定所需的行控制器类型以及他们排列的顺序。更多相关的信息，请参照 [WKInterfaceTable类参考](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceTable_class/index.html#//apple_ref/doc/uid/TP40014965).

## 配置行控制器

行控制器是用来显示你表格内单一行数据的模板。每个表格需要一个或者多个行控制器。比如，你可以用不同的行控制器来显示表格主要内容、表格头、表格尾。当添加一个新的表格到场景中时。Xcode会自动创建一个初始化的行控制制器,稍后你可以添加更多其他的行控制器.

**给表格添加行控制器**

1. 在Storyboard中选中表格对象.
2. 打开属性探测器(Attributes inspector).
3. 用Rows属性来改变可用的行控制器的数量.
   
每一个行控制器都有一些初始化元素。你可以把你需要的，比如标签、图片或者其他对象添加到行中。标签和图片的内容在设计的时候是无关紧要的。在运行时，你可以在对行进行配置的时候把内容替换掉。

每一个行控制器都由一个自定义类支持，你可以用这个类来接入行的内容。大部分行控制器都只包含与行的UI控件相关的属性,基本上是没有代码的。然而，当你给行添加按键或者其他可交互的控件时，你的行控制器的相关类也可以包含对这些交互事件响应的相关方法。

**给行控制器定义自定义类**    

1. 添加一个Cocoa Touch 类到你的WatchKit扩展中
2. 让你新创建的类继承自[NSObject](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/index.html#//apple_ref/occ/cl/NSObject)。
3. 把运行时需要接入的属性，如标签、图片等添加到类中。确保这些属性使用以下的格式，保证属性的类型和对应的界面上的控件类型相同：
   
```
@property (weak, nonatomic) IBOutlet WKInterfaceLabel *label; //Objective-C
```

以下是一个简单的行控制器类的定义，这个例子中，类包含了一个图片和一个标签的输入口。

```
SWITT
class MainRowType: NSObject {
    @IBOutlet weak var rowDescription: WKInterfaceLabel!
    @IBOutlet weak var rowIcon: WKInterfaceImage!
}
```

```
OBJECTIVE-C
@interface MainRowType : NSObject
@property (weak, nonatomic) IBOutlet WKInterfaceLabel* rowDescription;
@property (weak, nonatomic) IBOutlet WKInterfaceImage* rowIcon;
@end    
```

创建类并且连接输入口到对应控件上后，你的行控制器的配置就完成了。你必须给行分配一个标示性字符，以便在创建行时使用。

**在storyboard中配置行控制器**   

1. 在storyboard中，选中行控制器对象。
2. 给行控制器设置一个唯一的标示性标签。稍后创建表格行的时候会用到标示。这个值必须确保在行类型中是唯一的，除此之外没有其他限制。在属性探测器中设置这个标示。
3. 把这个行控制器的类设成你的自定义类。并在Identifty Inspector中设置标示。
4. 创建并连接标签以及其他元素的输入口，运行时,WatchKit需要这些信息来创建行的界面对象。
   
图 10-1 展示了一个由代码10-1定义的标示为mainRowType，类为MainRowType的行控制器。输入口rowDescription和rowIcon分别连接到行中的图片和标签对象。

**图 10-1** 检查Xcode中的行控制器    

![figure10-1](../images/table_row_definition_2x.png)

## 运行时配置表格内容

运行时，你可以动态添加行到表格中，并且配置它的内容。add and configure rows as part of your interface controller's initialization process.

**创建并配置表格中的行**  

1. 基于你需要展示的数据来确定行的数量和类型
2. 使用[setRowTypes:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceTable_class/index.html#//apple_ref/occ/instm/WKInterfaceTable/setRowTypes:)和[setNumberOfRows:withRowType:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceTable_class/index.html#//apple_ref/occ/instm/WKInterfaceTable/setNumberOfRows:withRowType:)方法来创建行。两个方法都可以在WatchKit扩展中创建行并且实例化行所对应的类。实例化后的类储存在表格中，可以通过[rowControllerAtIndex:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceTable_class/index.html#//apple_ref/occ/instm/WKInterfaceTable/rowControllerAtIndex:)方法接入。
3. 使用rowControllerAtIndex:方法来对行进行迭代。
4. 使用行控制器对象来配置行的内容
   
`setRowTypes:` 和 `setNumberOfRows:withRowType:` 方法对对应的行控制器相关联的类进行实例化。调用这两个方法后，会立即获取最新创建的行控制器对象并用它来配置行。代码 10-2 展示了一个简单的使用提供的数据对行的标签和图片进行配置的例子。数据由一个自定义数据类型 MyDataObject 组成的数组提供。（`MyDataObject` 类有一个字符串和一个图片属性，它的实现这边没有展示出来）行本则是自定义类型 `MainRowType` 类的实例。

**代码 10-2** 创建并配置表格的各行   

```
- (void) configureTableWithData:(NSArray *) dataObjects{
    [self.table setNumberOfRows:[dataObjects count] withRowType:@"mainRowType"];
    for(NSInteger i = 0; i < self.table.numberOfRows; i++){
        MainRowType *theRow = [self.table rowControllerAtIndex:i];
        MyDataObject *dataObj = [dataObjects objectAtIndex:i];
        [theRow.rowDescription setText:dataObj.text];
        [theRow.rowIcon setImage:dataObj.image];
   }
}
```
   
配置表格时，你可以限制初始化时行的数量来提高性能。由于表格的行都是必须在前台创建的, 如果创建的数量过大会严重影响你应用的性能.具体的数量限制取决于你数据的复杂程度以及创建行需要的时间，建议把总数量限制在20以下。对于需要更多行的表格，可以考虑在初始化的时候只加载一部分行，并提供给用户可控制的展示更多行的方法。更好的解决方案是指显示最重要的那几行。比如，你的数据为定位数据的时候，把数据限制到和用户当前位置相关的那几行。

## 处理行选择

界面控制器可以用来处理它所持有的任意表格的行选择事件。当用户点击表格上的某一行时，WatchKit会选中那行并调用控制器的[table:didSelectRowAtIndex:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/instm/WKInterfaceController/table:didSelectRowAtIndex:)方法。使用这个方法来执行相关的行为。比如，你可能需要展示一个新的界面控制器或者刷新行的内容。如果你不需要某一行处于可选状态，可以在storyboard中把对应的行控制器的Selectable选项取消。
   