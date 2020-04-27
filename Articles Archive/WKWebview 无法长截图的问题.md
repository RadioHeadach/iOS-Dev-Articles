# WKWebview 无法长截图的问题

文章曾发布于简书：https://www.jianshu.com/p/7ad6e404a46b

因为业务需求，要做一个长截图的功能，又因为项目是基于`WKWebview`开发的项目，所以长截图的需求就变成了对`WKWebview`进行长截图。

在网上搜索了一番，发现了一套长截图的逻辑，如下：

```Objective-c
- (void)snapshotForWKWebView:(WKWebView *)webView CaptureCompletionHandler:(void(^)(UIImage *capturedImage))completionHandler
{
    // 1.为了截图时对 frame 进行操作不会出现闪屏等现象，我们需要盖一个“假”的 webView 到现在的位置上，并将真正的 webView “摘下来”。调用 snapshotViewAfterScreenUpdates 即可得到这样一个“假”的 webView
    UIView *snapshotView = [webView snapshotViewAfterScreenUpdates:YES];
    snapshotView.frame = webView.frame;
    [webView.superview addSubview:snapshotView];

    // 2. 保存真正的 webView 的偏移、位置等信息，以便截图完成之后“还原现场”
    CGPoint currentOffset = webView.scrollView.contentOffset;
    CGRect currentFrame = webView.frame;
    UIView *currentSuperView = webView.superview;
    NSUInteger currentIndex = [webView.superview.subviews indexOfObject:webView];

    // 3. 用一个新的视图承载“真正的” webView，这个视图也是绘图所用到的上下文
    UIView *containerView = [[UIView alloc] initWithFrame:webView.bounds];
    [webView removeFromSuperview];
    [containerView addSubview:webView];

    // 4. 将 webView 按照实际内容高度和屏幕高度分成 page 页
    CGSize totalSize = webView.scrollView.contentSize;
    NSInteger page = ceil(totalSize.height / containerView.bounds.size.height);

    webView.scrollView.contentOffset = CGPointZero;
    webView.frame = CGRectMake(0, 0, containerView.bounds.size.width, webView.scrollView.contentSize.height);

    UIGraphicsBeginImageContextWithOptions(totalSize, YES, UIScreen.mainScreen.scale);
    [self drawContentPage:containerView webView:webView index:0 maxIndex:page completion:^{
        UIImage *snapshotImage = UIGraphicsGetImageFromCurrentImageContext();
        UIGraphicsEndImageContext();

        [webView removeFromSuperview];
        [currentSuperView insertSubview:webView atIndex:currentIndex];
        webView.frame = currentFrame;
        webView.scrollView.contentOffset = currentOffset;

        // 8. 调用 UIGraphicsGetImageFromCurrentImageContext 方法从当前上下文中获取到完整截图，将第 2 步中保存的信息重新赋予到 webView 上，“还原现场”
        [snapshotView removeFromSuperview];

        completionHandler(snapshotImage);

    }];
}

- (void)drawContentPage:(UIView *)targetView webView:(WKWebView *)webView index:(NSInteger)index maxIndex:(NSInteger)maxIndex completion:(dispatch_block_t)completion
{
    // 5. 得到每一页的实际位置，并将 webView 往上推到该位置
    CGRect splitFrame = CGRectMake(0, index * CGRectGetHeight(targetView.bounds), targetView.bounds.size.width, targetView.frame.size.height);
    CGRect myFrame = webView.frame;
    myFrame.origin.y = -(index * targetView.frame.size.height);
    webView.frame = myFrame;

    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(0.1 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        // 6. 调用 drawViewHierarchyInRect 将当前位置的 webView 渲染到上下文中
        [targetView drawViewHierarchyInRect:splitFrame afterScreenUpdates:YES];

        // 7. 如果还未到达最后一页，则递归调用 drawViewHierarchyInRect 方法进行渲染；如果已经渲染完了全部页，则回调通知截图完成
        if (index < maxIndex) {
            [self drawContentPage:targetView webView:webView index:index + 1 maxIndex:maxIndex completion:completion];
        } else {
            completion();
        }
    });
}

```

这套截图代码一定要获取`webView.scrollView.contentSize`，也就是页面的总高度，但项目内页面使用的`framework7`的css里面写了
```css
body{
  height : 100%;
}
```
因此获取页面的总高度都是等于屏幕的高度，导致一直无法截图，我尝试用KVO监听，也尝试过`[self.webView evaluateJavaScript:@"document.body.scrollHeight"]`，所获取的页面高度都一直等于屏幕高度，而不等于页面的实际高度。因此这套代码在我的项目上无法实现长截图。

[返回README](https://github.com/RadioHeadach/iOS-Dev-Articles)