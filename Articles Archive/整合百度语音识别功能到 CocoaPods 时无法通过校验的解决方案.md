# 整合百度语音识别功能到 CocoaPods 时无法通过校验的解决方案

文章曾发布于简书：https://www.jianshu.com/p/02ff60ff75a6

# 目录
1. 出现的问问题
2. 解决思路
3. 解决方法

---

# 1 出现的问题

由于工作需要，我要把`百度语音识别SDK`整合到 CocoaPods 内，再开发完要进行校验的时候出现了如下问题：

![图1: 进行 CocoaPods 检验时出现的问题](https://upload-images.jianshu.io/upload_images/187394-8e3e1248863fc7d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 2 解决思路
由图中可以看到，问题出在 `libBaiduSpeechSDK.a` 这个二进制文件，在 Google 上搜索了这个文件，发现也有其他人遇到了这样的问题 https://www.jianshu.com/p/fea0a24def1c 

![来源：https://www.jianshu.com/p/fea0a24def1c](https://upload-images.jianshu.io/upload_images/187394-a7a4a9407f000d75.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
文章作者提供的解决方案是 
>将libstdc++.dylib换成libstdc++.6.0.9.dylib即可

但该作者问题里面缺失的编译与我的不同，于是我直接搜索我确实的编译标志，得到如下的结果：
http://ask.dcloud.net.cn/question/4040

# 3 解决方案 

添加CoreTelephony.framework，添加方法为：

![image.png](https://upload-images.jianshu.io/upload_images/187394-c21c9a3bd599db79.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在 podspec 文件的 `s.framework` 后面加上 `"CoreTelephony"`。

完毕。

[返回README](https://github.com/RadioHeadach/iOS-Dev-Articles)