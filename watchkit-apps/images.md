# 图片　　

WatchKit 提供了下列方法把图片合并到内容：

- [WKInterfaceImage](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceImage_class/index.html#//apple_ref/occ/cl/WKInterfaceImage) 表示一个图像或图像序列。　　
- [WKInterfaceGroup](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceGroup_class/index.html#//apple_ref/occ/cl/WKInterfaceGroup)，[WKInterfaceButton](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceButton_class/index.html#//apple_ref/occ/cl/WKInterfaceButton) 和 [WKInterfaceController](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/cl/WKInterfaceController) 类允许您指定一个图像作为其他内容的背景。

## 指定图像资产　　　　

当创建图像资产时请遵循以下建议：

- 尽量使用 PNG 格式的图像。
- 请创建大小适当的图像。对于无法控制尺寸的图像，请使用 [setWidth:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceObject_class/index.html#//apple_ref/occ/instm/WKInterfaceObject/setWidth:) 和 [setHeight:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceObject_class/index.html#//apple_ref/occ/instm/WKInterfaceObject/setHeight:) 接口对象的方法，以确保图像以合适尺寸显示。
- 使用图像资产来管理图像。图像资产允许为各设备尺寸指定不同图像版本。

## 使用命名图像以提高性能　

下列方法可以改变接口对象的当前图像：

- 使用 [setImageNamed:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceImage_class/index.html#//apple_ref/occ/instm/WKInterfaceImage/setImageNamed:) 或者 [setBackgroundImageNamed:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceGroup_class/index.html#//apple_ref/occ/instm/WKInterfaceGroup/setBackgroundImageNamed:) 方法来配置存于 WatchKit 应用程序包或设备缓存中的图像。  
通过名称指定图片是首选方法，因为名称字符串会传输到 Apple Watch,这样会花费更少的时间和能量。WatchKit 会搜索 WatchKit 程序包找到指定名称的图像文件。如果未曾找到，就会搜索 device-side 图像缓存以查找指定名称的图像。
- 使用 [setImage:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceImage_class/index.html#//apple_ref/occ/instm/WKInterfaceImage/setImage:)，[setImageData:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceImage_class/index.html#//apple_ref/occ/instm/WKInterfaceImage/setImageData:)，[setBackgroundImage:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceGroup_class/index.html#//apple_ref/occ/instm/WKInterfaceGroup/setBackgroundImage:) 或 [setBackgroundImageData:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceGroup_class/index.html#//apple_ref/occ/instm/WKInterfaceGroup/setBackgroundImageData:) 方法把图像数据从 WatchKit 扩展无线传输到 WatchKit 应用程序。   

当在扩展中创建 `UIImage` 对象时，图像对象需保存在用户 iPhone 中，且使用前要发送给 Apple Watch。使用 [imageNamed:](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImage_Class/index.html#//apple_ref/occ/clm/UIImage/imageNamed:) 方法从 WatchKit 扩展包而不是 WatchKit 应用程序来加载图像。如果试图将该图像分配给接口对象，图像数据会通过无线传输到苹果手表。

## 缓存设备上的图像　　

对于在 WatchKit 扩展中创建的频繁使用图像，会在设备中根据名称缓存图像。在调用 `WKInterfaceDevice` 中的 [addCachedImage:name:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceDevice_class/index.html#//apple_ref/occ/instm/WKInterfaceDevice/addCachedImage:name:) 或 [addCachedImageWithData:name:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceDevice_class/index.html#//apple_ref/occ/instm/WKInterfaceDevice/addCachedImageWithData:name:) 方法使用图像之前需要进行图片缓存。　   

要使用接口中的缓存图片在你的界面，请执行以下操作：

- 针对 [WKInterfaceImage](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceImage_class/index.html#//apple_ref/occ/cl/WKInterfaceImage) 对象，请调用 `setImageNamed:` 方法来指定缓存图片的名称。　
- 针对 [WKInterfaceGroup](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceGroup_class/index.html#//apple_ref/occ/cl/WKInterfaceGroup) 和 [WKInterfaceButton](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceButton_class/index.html#//apple_ref/occ/cl/WKInterfaceButton) 对象，请调用 `setBackgroundImageNamed:` 方法来指定缓存图片的名称。

>
**重要**   
当缓存动画图片时，使用 [animatedImageWithImages:duration:](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImage_Class/index.html#//apple_ref/occ/clm/UIImage/animatedImageWithImages:duration:) 方法用所有的动画帧和缓存图片来创建一个单独的 `UIImage` 对象来。不要用单独地个性的帧来缓存图片。

Apple Watch 图像缓存尺寸受限，每个应用程序拥有大约 5 MB 的缓存空间。当应用程序占用缓存后，添加新图像之前就必须从缓存中移除现有图像。使用 [removeCachedImageWithName:](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceDevice_class/index.html#//apple_ref/occ/instm/WKInterfaceDevice/removeCachedImageWithName:) 方法来删除单个图像或使用 [removeAllCachedImages](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceDevice_class/index.html#//apple_ref/occ/instm/WKInterfaceDevice/removeAllCachedImages) 方法来彻底清除缓存。
