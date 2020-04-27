# CocoaPod 本地校验出现 i386 错误

文章曾发布于简书：https://www.jianshu.com/p/205d40f875b3

# 目录
1. 出现的问题
2. 解决方法
3. 延伸

## 1. 出现的问题
由于公司是组件化开发，每个组件都是以 CocoaPod 的形式来依赖的，所以每次开发完成都要进行 pod 的本地校验，这次整合了一个静态库，但是在校验的时候无法通过，原因如下：

```
 Undefined symbols for architecture i386:
      "_sp_font_create_base_font", referenced from:
          _KGPDFPDFAddWaterMarkToPage in libiAppPDF.a(KGPDFPage.o)
          _KGPDFPDFAddWaterMarkToDocument in libiAppPDF.a(KGPDFPage.o)
        
        (此处省略一大堆报错)

 ld: symbol(s) not found for architecture i386
    clang: error: linker command failed with exit code 1 (use -v to see invocation)

    ** BUILD FAILED **
```

但是通过 `lipo -info` 命令发现其实该 SDK 是支持 i386 架构的，因此陷入困境。

## 2. 解决方法
我通过在 podspec 里面设置来绕过 `i386` 模拟器架构的编译。在 podspec 文件内加入以下代码：

```
  s.pod_target_xcconfig = { 'VALID_ARCHS' => 'arm64 armv7 armv7s x86_64' }
```

## 3. 延伸

[iOS 中的 armv7,armv7s,arm64,i386,x86_64 都是什么]([https://www.jianshu.com/p/3fce0bd6f045](https://www.jianshu.com/p/3fce0bd6f045)
)

[iOS开发之CocoaPods：进阶篇 搭建私有库]([https://kanggggg.github.io/2019/01/25/iOS%E5%BC%80%E5%8F%91%E4%B9%8BCocoaPods%EF%BC%9A%E8%BF%9B%E9%98%B6%E7%AF%87-%E6%90%AD%E5%BB%BA%E7%A7%81%E6%9C%89%E5%BA%93/index.html](https://kanggggg.github.io/2019/01/25/iOS%E5%BC%80%E5%8F%91%E4%B9%8BCocoaPods%EF%BC%9A%E8%BF%9B%E9%98%B6%E7%AF%87-%E6%90%AD%E5%BB%BA%E7%A7%81%E6%9C%89%E5%BA%93/index.html)
)

[解决pod lib lint/repo push不支持i386编译&只能真机运行的库]([https://www.jianshu.com/p/88180b4d2ab7](https://www.jianshu.com/p/88180b4d2ab7)
)

[返回README](https://github.com/RadioHeadach/iOS-Dev-Articles)