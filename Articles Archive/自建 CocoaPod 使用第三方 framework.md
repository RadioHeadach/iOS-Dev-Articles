# 自建 CocoaPod 使用第三方 framework

文章曾发布于简书：https://www.jianshu.com/p/173fceecb43c

在使用 CocoaPod 写组件时，需要用到第三方库，对此，`podspec` 文件应该这样写：

```
  s.vendored_frameworks = 'Library/*.framework'
```

我的文件目录如图：

![文件目录](https://upload-images.jianshu.io/upload_images/187394-27d6e71e2ebffbc5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
