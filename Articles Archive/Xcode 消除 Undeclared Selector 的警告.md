# Xcode 消除 Undeclared Selector 的警告

文章曾发布于简书：https://www.jianshu.com/p/fd900d4db216

![警告](https://upload-images.jianshu.io/upload_images/187394-5f1ee371c93e2c66.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在开发的时候可能会遇到这样的问题，调用一些 SDK 的私有方法，但是会被 Xcode 警告方法不存在，这对于一个强迫症患者而言是不可接受的，于是我们可以手动让这个警告消失。

## 处理方法

### 第一步 获得错误名

![reveal in log](https://upload-images.jianshu.io/upload_images/187394-e501a8d3f22ccb44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对这个错误点击右键，选择 Reveal in Log。

![获得错误名](https://upload-images.jianshu.io/upload_images/187394-35f47f07e8ec4651.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 第二步 消除错误名

![消除错误](https://upload-images.jianshu.io/upload_images/187394-4ee124b2397ddb86.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```objc
// 清除警告
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Wundeclared-selector" // 双引号内是你在上一步获得的错误名
// 需要被消除⚠️警告的代码写在这里
#pragma clang diagnostic pop
```

当然这个方法不仅仅可以消除`Undeclared Selector`这个警告，其他的警告也可以消除。

### 延伸

[nshipster 关于 pragma 的介绍](https://nshipster.cn/pragma/)

[返回README](https://github.com/RadioHeadach/iOS-Dev-Articles)