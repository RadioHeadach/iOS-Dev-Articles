# 牛客网iOS开发笔试题选择题总结 | Objective-C

文章曾发布于简书：https://www.jianshu.com/p/0bc92a9c8420

# 笔试总结


## import””, import<>, 的区别
1· `#import`和`#include`的区别：
    import不会引发交叉编译，include会
`#import<>`和`#import“”`的区别：
    `import<>`引用系统文件，对于系统自带头文件的饮用
    `import“”`引用自定义文件，首先会在用户目录下查找

## 视图相关
1· 没有navigationController的情况下，从一个viewController切换到另一个viewController的方法是：
    `[self presentModalViewController:nextViewController animated:YES]`
     `pushViewController`需要有navigationController

- - -
2· UISplitViewController控制器可以呈现屏幕分栏视图的效果，MasterView占有320点的固定大小。
正确答案: B
```
A 是
B 否 
```
解释：
ios8 sdk 中  ，UISplitViewController 可以在 iphone 中 使用 了，masterView 的 宽度 是 整个 屏幕的 宽度，所以 不是 固定的 大小 320
- - -

3· iOS中导航设计模式有几种？
正确答案: A B C   你的答案: A B C D (错误)

```
A 平铺导航
B 标签导航
C 树形导航
D 模态视图导航
```
解释：
```
我们经常会遇到在某个路径中滑出一个单屏、进行编辑、查看信息、操作界面的上的内容的情况发生。这是一种应用行为的特定形态，一般带有流程的界面变更的情况发生，比如一张页面临时取代了整个应用程序的显示屏，我们称这种处理方式为“模态视图”。默认情况下，模式视图从屏幕底部边缘滑上来切一半覆盖了当前整个屏幕,模态视图完成和程序主功能有关系的独立任务，尤其适合于主功能界面中欠缺的多级子任务。这种操作会暂时绕开应用的正常操作。

模态视图常常被用来编辑或添加内容，当你需要的时候模态视图一般从屏幕底部滑出而后遮盖先前的页面，当你完成任务后滑出的页面也会相应的缩回去，然后可以继续之前的流程。有些控件和界面元素只在次要任务中被偶尔用到，模态视图很好的把他们暂时隐藏了，并且当需要的时候出现，有效的节约了屏幕空间。

模态视图有点像是导航中的死胡同，为了能够让用户也可以同样方便的回到正常的流程中去，模态视图除了正常的操作之外一般还有加上一个“完成”按钮，或者“取消”按钮。
```
- - -

4· 模态视图专用属性有哪些？
正确答案: A B C D 
```
A UIModalPresentationFullScreen，全屏状态，是默认呈现样式，iPhone只能全屏呈现。
B UIModalPresentationPageSheet，它的宽度是固定的768点，在iPad竖屏情况下则全屏呈现。
C UIModalPresentationFormSheet，它的是固定的540x620点，无论是横屏还是竖屏情况下呈现尺寸都不会变化。
D UIModalPresentationCurrentContext，它与父视图控制器有相同的呈现方式。 
```
解释：
```
作用：
临时中断当前工作流程，显示一个新的视图层次结构。
用途：
立即从用户那里收集信息；
临时显示一些内容；
临时改变工作模式；
为不同的设备方向实现可替代的界面；
使用指定类型的过渡动画来显示一个新的视图结构；
模态视图的显示风格：
通过设置属性modalpresentationStyle
UIModalPresentationFullScreen;
UIModalPresentationPageSheet;
UIModalPresentationFormSheet;
模态视图的过渡风格：
通过设置属性modalTransitionStyle
UIModalTransitionStyleCoverVertical;
UIModalTransitionStyleFlipHorizontal;
UIModalTransitionStyleCrossDissolve;
步骤：
创建一个要展示的视图控件；
在适当的地方分配一个委托对象；
调用当前视图控制前的presentModalViewController:animated:方法，传递你要模态显示的视图控制器；
```

## 多线程相关
1· 实现多线程都有哪几种方法？
正确答案: B C
```
A 使用@synchronized(self)
B 使用GCD
C 使用NSOperationQueue
D 使用@thread 
```
解释：`@thred`是安卓的关键字

2· 关于NSOperation queue的说法，正确的是？
正确答案: A C D 
```  
A 主要用于多线程并发处理
B 它是一个队列，有严格的先进先出
C 它不会遵守严格的先进先出
D NSOperationQueue可以通过调整权重来调整队列的执行顺序 
```
解释：
```
首先NSOperationQueue是属于多线程并发处理这一部分，它主要用来提供一个可添加的操作队列，将一系列操作添加到队列中，然后根据操作的优先级和内部操作依赖来决定操作执行的顺序。高优先级的操作先于低优先级执行。一个操作所依赖的操作全部执行完毕后才能执行。所以A,C,D正确
```

## 内存相关
 1· 与alloc相反，与retain相反，与alloc配对的分别是:
	 dealloc, release, release 
 
正确答案: B 
```
A dealloc release dealloc
B dealloc release release
C dealloc dealloc dealloc
D release release release
```
解释:
```
alloc其实相当于c++中的构造函数，dealloc相当于析构函数；retain相反的是release，这个应该没异议，alloc配对使用的也是release，alloc会使对象的retainCount=1，所以配对的是release。
```
---

 2· 下列程序输出是：
 ```
NSMutableArray* ary = [[NSMutableArray array] retain];
NSString *str = [NSString stringWithFormat:@"test"];
[str retain];
[ary addObject:str];
NSLog(@"%@%d",str,[str retainCount]);
[str retain];
[str release];
[str release];
NSLog(@"%@%d",str,[str retainCount]);
[ary removeAllObjects];
NSLog(@"%@%d",str,[str retainCount]);
 ``` 
 答案：
``` 
	-1, -1, -1 
```
解释：
NSString *str = [NSString **stringWithFormat**:@"test”];
当中`stringWithFormat`是类方法，内存管理上是autorelease的，因此内存管理是-1
- - -

3· 下面关于 Objective-C 内存管理的描述错误的是
正确答案: A C   你的答案: A B C (错误)
```
A 当使用ARC来管理内存时，代码中可以出现autorelease
B autoreleasepoll在drain的时候会释放在其中分配的对象
C 当使用ARC来管理内存时，在线程中大量分配对象而不用autoreleasepool则可能会造成内存泄露
```
- - - 

4· 关于内存管理，下列说法错误的是
正确答案: C  

```
A 谁申请，谁释放
B 内存管理主要要避免“过早释放”和“内存泄漏”，对于“过早释放”需要注意@property设置特性时，一定要用对特性关键字，对于“内存泄漏”，一定要申请了要负责释放。
C 关键字alloc 或new 生成的对象可以自动释放；
D 设置正确的property属性，对于retain需要在合适的地方释放， 
```
解释:
```
alloc或new生成的对象不会自动释放，要自动释放需要调用autorelease方法。

谁申请，谁释放。也不是很对，在non-ARC中，如果一个函数要返回一个在堆上alloc的对象，在返回的时候需要调用autorelease方法。实际上是你申请了，但释放交给了NSAutoReleasePool。
```


##  RunLoop相关
1· 选C
```
NSRunLoop的以下描述错误的是（） 1/1
A Runloop并不是由系统自动控制的
B 有3类对象可以被run loop监控：sources，timers，observers
C 线程是默认启动run loop的
D NSTimer可手动添加到新建的NSRunLoop中
```
解释：
```
A:Runloop的作用在于当有事情要做时它使当前的thread工作，没有事情做时又使thread 休眠sleep。Runloop并不是由系统自动控制的，尤其是对那些新建的次线程需要对其进行显示的控制。

B：有3类对象可以被run loop监控：sources、timers、observers。当这些对象需要处理的时候，为了接收回调，首先必须通
过 CFRunLoopAddSource ,CFRunLoopAddTimer 或者 CFRunLoopAddObserver 把这些对象放入run loop。 要停止接收它的回调，可以通过CFRunLoopRemoveSource从run loop中移除某个对象。 

C：每一个线程都有自己的runloop, 主线程是默认开启的，创建的子线程要手动开启，因为NSApplication 只启动main applicaiton thread。

D：NSTimer默认添加到当前NSRunLoop中，也可以手动制定添加到自己新建的NSRunLoop的中。
```

2· 对下述代码错误描述正确的是( )
```
NSTimer *myTimer = [NSTimer timerWithTimeInterval:1.0 target:self selector:@selector(doSomeThing:) userInfo:nil repeats:YES]; 
[myTimer fire]
```
正确答案: A
```
A 没有将timer加入runloop
B doSomeThing缺少参数
C 忘记传递数据给userInfo
D myTimer对象未通过[[myTimer alloc] init]方法初始化
```
解释：
NSTimer有两种初始化方法：
初始化方法一： `scheduledTimerWithTimeInterval:target:selector:userInfo:repeats: 方法`

```
+ (NSTimer *)scheduledTimerWithTimeInterval:(NSTimeInterval)seconds target:(id)target selector:(SEL)aSelector userInfo:(id)userInfo repeats:(BOOL)repeats
 
seconds ：需要调用的毫秒数
target ：调用方法需要发送的对象。即：发给谁
userInfo ：发送的参数
repeats ：指定定时器是否重复调用目标方法
 
现在做个比喻：
可以把调度一个计时器与启动汽车的引擎相比较。被调度的计时器就是运行中的引擎。没有被调度的计时器就是一个已经准备好启动但是还没有运行的引擎。我们在程序里面 , 无论何时 , 都可以调度和取消调度计时器 , 就像根据我们所处的环境 , 决定汽车的引擎是启动还是停止。

如果你想要在程序中 , 手动的在某一个确定时间点调度计时器 , 可以使用 NSTimer 的类方法 timerWithTimeInterval:target:selector:userInfo:repeats: 方法。
```
初始化方法二：
```
如果，我们想在任何时候都能够随心所欲的 启动 / 停止定时器。
咋办？不用急，还有 NSTimer 的另一种初始化方法，能够满足我们的要求：
// 使用 timerWithTimeInterval 方法来实例化一个 NSTimer, 这时候 NSTimer 是不会启动的
self.paintingTimer = [NSTimer timerWithTimeInterval:1.0
                                                 target:self
                                               selector:@selector(paint:)
                                               userInfo:nil
                                                repeats:YES];
 
 
// 当需要调用时 , 可以把计时器添加到事件处理循环中
 [[NSRunLoop currentRunLoop] addTimer:self.paintingTimer forMode:NSDefaultRunLoopMode];
```
总结： `scheduled`开头的方法初始化的，会将这个timer调度到当前运行的loop中; timer和init开头的初始化方法，只是创建，并没有调度到loop中，需要手动addTimer。

3· 
```
NSString *str = @“lanou”;
[str retain];
NSLog(@“%lu”,str.retainCount);
```
此处打印出来的值是多少().
正确答案: D
```
A 1
B 2
C -1
D ULONG_MAX
```
解释：
这是一个放在常量区的字符串常量，返回的结果是UINT_MAX值  有些地方的回答

## TableView相关

1· 以下关于tableView编辑的方法中哪个不属于代理方法？**A** 
``` 
A -(void)setEditing:(BOOL)editing animated:(BOOL)animated

B -(BOOL)tableView:(UITableView *)tableView canEditRowAtIndexPath:(NSIndexPath *)indexPath

C -(UITableViewCellEditingStyle)tableView:(UITableView *)tableView editingStyleForRowAtIndexPath:(NSIndexPath *)indexPath

D -(void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath
```
解释： A是tableView自己的方法，不是代理
- - -

2· 下面哪些属于UITableViewDelegate协议的方法？
正确答案: C   
```
A tableView:cellForRowAtIndexPath:
B tableView:numberOfRowsInSection:
C tableView:didSelectRowAtIndexPath:
D numberOfSectionsInTableView:
```
解释：
代理的作用是用来完成指定的某种动作，所以必须是动作性的操作而不是数据性的操作


## property相关

1· 关于readwrite, readonly, assign, automatic的说法，下列说法错误的是
```
A readwrite 是可读可写特性；需要生成getter方法和setter方法时
B readonly 是只读特性 只会生成getter方法 不会生成setter方法 ;不希望属性在类外改变
C assign 是赋值特性，setter方法将传入参数赋值给实例变量；仅设置变量时；
D nonatomic 非原子操作，决定编译器生成的setter getter是否是原子操作；nonatomic表示多线程安全；一般使用atomic
```

解释：

D项说一般使用atomic是不对的，一般使用nonatomic。

readonly, readwrite：是控制属性的访问权限，readonly只生成getter方法，其他类是无法修改其值的。readwrite是会同时生成getter和setter方法，其他类可以修改其值。

assign, retain, weak, strong, copy, unsafe_unretained：
在non-ARC中，assign和retain是一组，assign的对象属性引用计数不变，而retain会被+1。
对应的在ARC中，weak和strong是一组，**weak的对象属性引用计数不变，而strong会被+1。**

**assign还用来赋值非对象属性**，例如，int，double，BOOL，char等。
**copy用来设置不可变的对象属性**，例如，NSString，NSArray，NSDictionary等。

在ARC中，一个属性被设置为weak，当出了作用域，其值会被设置成nil。与其相对应的是unsafe_unretained，unsafe_unretained不会改变对象属性的引用计数，同时出了作用域的时候，其值也不会被设置成nil。unsafe_unretained相当于non-ARC中的assign。

atomic, nonatomic: atomic是原子操作，nonatomic是非原子操作，一般常用的是nonatomic。

2· 设置代理为属性正确的是（）
正确答案: A 
```
A @property(nonatomic,assign)
B @property(atomic,copy)
C @property(nonatomic,copy)
D @property(nonatomic,retain)
```
解释：
用weak更好，但是A最接近

3· 关于Objective-C中属性的说明，以下错误的是（）
正确答案: D 
```
A readwrite是可读可写特性，需要生成getter方法和setter方法
B readonly是只读特性，只有getter方法，没有setter方法
C assign是赋值属性，setter方法将传入参数赋值给实例变量
D retain表示持有特性，copy属性表示拷贝属性，都会建立一个相同的对象
```
解释：
`copy`是直接创建一个 

- - -

## 数组、字典等相关

1· 以下哪一段代码不会抛出异常（） **C**
```
A NSArray *array=@[1，2，3];NSNumber * number=array[3];

B NSDictionary *dict=@{@"key":nil};

C NSString *str=nil;NSString *str2=[str substringFromIndex:3];

D NSString *str=@"hi";NSString *str2=[str substringFromIndex:3];
```

解释：
```
A 数组越界
B 字典的Key不能为nil
D 数组下标越界
```

- - - 
2· 关于下列程序，输出是
```
NSMutableArray* ary = [[NSMutableArray array] retain];
NSString *str = [NSString stringWithFormat:@"test"];
[str retain];
[ary addObject:str];
NSLog(@"%@%d",str,[str retainCount]);
[str retain];
[str release];
[str release];
NSLog(@"%@%d",str,[str retainCount]);
[aryremoveAllObjects];
NSLog(@"%@%d",str,[str retainCount]);
```
正确答案: D 
```
A 2，3，1
B 3，2，1
C 1，2，3
D -1，-1，-1
```
解释：
```
objective-C: NSString应该用initWithFormat? 还是 stringWithFormat?

今天在看书上的一段代码时，发现NSString实例化时，有时用的是initWithFormat方法，有时用的是stringWithFormat，到底应该如何选择呢？

区别：

1、initWithFormat是实例方法

只能通过 NSString* str = [[NSString alloc] initWithFormat:@"%@",@"Hello World"] 调用，但是必须手动release来释放内存资源

2、stringWithFormat是类方法

可以直接用 NSString* str = [NSString stringWithFormat:@"%@",@"Hello World"] 调用，内存管理上是autorelease的，不用手动显式release。

```


## 网络相关

1· NSURLConnectionDelegate协议中的方法有哪些？
正确答案: A B D
```
A connection:didReceiveData:
B connection:didFailWithError:
C initWithRequest:delegate:
D connectionDidFinishLoading:
```

解释：
```
单 看 NSURLConnectionDelegate ，答案只有 B ，而 A 和 D 属于 NSURLConnectionDataDelegate 。
仔 细 看 NSURLConnectionDataDelegate 的声明：
@protocol NSURLConnectionDataDelegate <NSURLConnectionDelegate>

C是方法，不是代理
```
- - -
2· NSURLConnection类的同步请求方法是？
正确答案: A 
```
A + sendSynchronousRequest:returningResponse:error:
B – initWithRequest:delegate:
C – initWithRequest:delegate:startImmediately:
```
解释：B和C都是异步方法，需要设置delegate属性。

3· NSURL的构造函数有？
正确答案: C D 
```
+ requestWithURL:
– initWithURL:
+ URLWithString:
– initWithString:
```
解释：
```
[NSURL URLWithString:@"http://a.com"]; 

[[NSURL alloc] initWithString:@"http://a.com"];
```


## 数据持久化相关

1· iOS中持久化方式有哪些？
正确答案: A B C D
```
A 属性列表文件
B 对象归档
C SQLite数据库
D CoreData
```

解释：
```
A ：属性列表文件   //NSUserDefaults 的存储，实际是本地生成一个 plist 文件，将所需属性存储在 plist 文件中
B ：对象归档     // 本地创建文件并写入数据，文件类型不限
C ： SQLite 数据库 // 本地创建数据库文件，进行数据处理
D ： CoreData    // 同数据库处理思想相同，但实现方式不同
某些情况下，B生成的文件也可以是数据库文件，但是对数据的处理方式肯定是不同的
所以，ABCD都属于本地持久化的方式
```
- - -
2· 以下适合在客户端做数据持久化存储的数据的有
正确答案: B D 
```
A redis
B localStorage
C sessionStorage
D userData
```
解释：
```
redis是一个开源使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库。所以A选项中redis数据就是一个干扰项，直接排除。

localStorage和sessionStorage一样都是用来存储客户端临时信息的对象。

不同：localStorage生命周期是永久，这意味着除非用户显示在浏览器提供的UI上清除localStorage信息，否则这些信息将永远存在。

sessionStorage生命周期为当前窗口或标签页，一旦窗口或标签页被永久关闭了，那么所有通过sessionStorage存储的数据也就被清空了。

所以选B不选C

D选项用户数据当然适合做持久化存储，免登陆就是获取之前存储的用户数据来实现的。
```

## 指针相关

1· 关于浅复制和深复制的说法，下列说法正确的是
正确答案: A B D
```
A 浅层复制：只复制指向对象的指针，而不复制引用对象本身。
B 深层复制：复制引用对象本身。
C 如果是浅复制，若类中存在成员变量指针，修改一个对象一定会影响另外一个对象
D 如果是深拷贝，修改一个对象不会影响到另外一个对象
```
解释：
我认为浅拷贝是一个不喜欢思考的懒汉，而深拷贝则是一个思维严谨，喜欢思考的人。对于懒汉来说，虽然给了他任务，但是他总是想尽量的少做一些事情，所以很多时候做出来的东西就是只看到了表面，不会去思考对不对。 

	structX{intx;inty;};

对于懒汉来说，他很直白的看到了x，看到了y，然后就拷贝x和y，然后就不管了，反正我完成我的拷贝了，至于对不对，我不管。

而一旦有了引用或者指针，事情就不一样了

	structX{intx;inty;int*p;};

懒汉依然只是直接表面级别的拷贝，于是拷贝x, y , p，但是他没有思考接下来的事情对不对。对于指针或者引用来说，若是只是拷贝表面，那么拷贝后的物体的指针也和原来的指针指向的是同一个对象，所以虽然目的想完成一个完美的克隆体，但是却发现克隆体和原来的物体中间还有一根线连着，没有完美的分离。

	int*p=newint(47);int*q=p;

如q与p都是指向一个物体一样。

那么如果原来的物体销毁了，但是现在拷贝的物体还在，那么这时候你拷贝后的物体的成员指针就是一个悬挂指针，指向了不再存在的物体，那么你访问的话，那就不知道会发生什么了。

而对于深拷贝，这一个勤奋的人，他不会只做表面，他会把每一个细节都照顾好。于是，当他遇到指针的时候，他会知道new出来一块新的内存，然后把原来指针指向的值拿过来，这样才是真正的完成了克隆体和原来的物体的完美分离，如果物体比作人的话，那么原来的人的每一根毛细血管都被完美的拷贝了过来，而绝非只是表面。所以，这样的代价会比浅拷贝耗费的精力更大，付出的努力更多，但是是值得的。当原来的物体销毁后，克隆体也可以活的很好。

然而事实上是这个世界上大多都是懒汉，包括编程的人，编译器等，所以默认的行为都是浅拷贝，于是有时候你需要做一个勤奋的人，让事情做正确，自己去完成深拷贝所需要的事情。


## delegate、protocol相关

1· 使用protocol时，声明一组可选择实现与否的函数，需要在声明的前一行加上：

正确答案: B 
```
A @required
B @optional
C @interface
D @protocol
```
解释： 默认是`@required`,可以选择实现与否就是`@optional`

## 内省相关

内省（Introspection）是面向对象语言和环境的一个强大特性，Objective-C和Cocoa在这个方面尤其的丰富。内省是对象揭示自己作为一个运行时对象的详细信息的一种能力。这些详细信息包括对象在继承树上的位置，对象是否遵循特定的协议，以及是否可以响应特定的消息。NSObject协议和类定义了很多内省方法，用于查询运行时信息，以便根据对象的特征进行识别。

明智地使用内省可以使面向对象的程序更加高效和强壮。它有助于避免错误地进行消息派发、错误地假设对象相等、以及类似的问题。
- - -
1· 下面哪个方法不属于NSObject的内省(Introspection)方法
正确答案: C
```
A isMemberOfClass
B responseToSelector
C init
D isKindOfClass
```

解释:
```
内省是指能够揭示自己作为一个运行时对象所具有的信息的能力。对于NSObject对象来说，内省能力有，揭示对象的继承关系(isMemberOfClass,isKindOfClass)，是否遵守某个协议(confirmsToProtocol)，是否实现某个方法(respondsToSelector)
```

## main函数相关

1· ios 应用程序中， main 函数在最大程度上被使用，应用程序运行的一小部分工作由 AppMain 函数来处理，是否正确？
正确答案: B 
```
A 对
B 错
```
解释：
```
在iOS应用程序，main函数的作用是很少的。它的主要工作是控制UIKit framework。

在iPhone的应用程序中,main函数仅在最小程度上被使用,应用程序运行所需的大多数实际工作由UIApplicationMain函数来处理。

main例程只做三件事:
1.创建一一个自动释放池,
2.调用UIApplicationMain函数, 
3.释放自动释放池。
```

## 测试相关

1· iOS单元测试框架有哪些？
正确答案: A B C 
```
A OCUnit
B GHUnit
C OCMock
D 0NSXML
```
解释：
```
OCUnit和XCTest都是官方的测试框架，OCUnit已经过时被XCTest所取代。

GHUnit和OCMock都是第三方的测试框架，其官方地址分别为：https://github.com/gh-unit/gh-unit
https://github.com/erikdoe/ocmock
```

## 正则表达式相关

1· 下列正则表达式不能匹配”www.innotechx.com”的是：
正确答案: A D 
```
A ^w+.w+.w+$
B [w]{0,3}.[a-z]*.[a-z]+
C ^w.*com$
D [w]{3}.[a-z]{11}.[a-z]
```
解释：
```
首先来看A选项，^表示匹配字符串的开始，而 w 和 . 是没有特殊意义的，千万不要看错成是“\w（匹配字母或数字或下划线或汉字）”了，+ 表示重复一次或者多次，$是匹配字符串的结束。所以该选项会匹配 www.www.www（其中w可重复一次以上）。

B选项，[w]{0,3}限定符，表示将w重复0到3次，  “ . ”无特殊意义，* 表示重复任意次，包括零次，[a-z]表示匹配a到z的字母，所以就是匹配a到z中的某一字母任意次重复。后面这个相同意思，就是+号是重复一次以上。连起来看，是可以匹配选项的。

C选项，与上述选项相同，^先匹配字符串开始，然后包含一个 w ，和 " . "重复任意次，最后以com结尾。这个正则会匹配包含了 “wcom”的字符串，但是w要是开头，com要是结尾，所以可以匹配选项，大家可以自己试试看。

D选项，重复三次w，然后一个" . ",但是后面这里要重复11次a到z中的某一字母，数了下题目中选项只有9个字母，后面就不要看啦，已经错啦~
```

## 图片相关

1· 使用imageNamed方法创建UIImage对象时，与普通的init方法有什么区别？

正确答案: C 
```
A 没有区别，只是为了方便
B imageNamed方法只是创建了一个指针，没有分配其他内存
C imageNamed方法将图片加载到内存中后不再释放
D imageNamed方法将使用完图片后立即释放 
```

解释：
```
imageNamed是会把读取到的image存在某个缓存里面，第二次读取相同图片的话系统就会直接从那个缓存中获取，从某种意义上好像一种优化，但是imageNamed读取到的那个图片似乎不会因为Memory Warning而释放，所以用这个会导致在 内存不足 的时候闪退。简单的说imageNamed采用了缓存机制，如果缓存中已加载了图片，直接从缓存读就行了，每次就不用再去读文件了，效率会更高。 
```

[返回README](https://github.com/RadioHeadach/iOS-Dev-Articles)