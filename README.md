# android-compass
android-compass is a dev manual about Android Architecture,Third Libs ,Utils and Some Solutions to dev.
# [README OF ENGLISH](https://github.com/BryceLee/android-compass/blob/master/README_EN.md)

## The Basis of Computer
- 操作系统
    - 多进程
    - 多线程
- 算法和数据结构 [Algorithms]
    - HaspMap
    - LinkedList
    - 快排
    - 最大堆
- 计算机网络
    - TCP/UDP
    - Http/Http2
    - QUIC


## 设计思想
- 什么是控制反转？
    - 可以这么理解，对象的创建交给一个“仓库”来管理（这里所谓的反转），而不是使用方自行创建（这就是正转）
    - 实现方式：
        - DI(Depency Injection),利用这些思想的库：Dagger2,Spring
        - Dependency Lookup
    - Thanks:
        - [IOC1](http://sishuok.com/forum/blogPost/list/2427.html)
        - [IOC2](https://www.cnblogs.com/maxstack/p/7516097.html)
- AOP
## 设计原则
![](https://cdn-images-1.medium.com/max/1600/1*ykdDqm06KRI1XDtv34b2BQ.png)
- 单一职能原则
    - 一个类只有一个使其改变的原因（我的理解是一个类一个功能）
    - 优点：降低耦合；代码易理解和维护；
- 开闭原则
    - 软件实体（类，模块，方法）应该开放扩张，不允许修改
    - 优点：更易维护和复用，更高健壮性
- 里氏替换原则（Liskov Substitution Principle）
    - 指的是对象被其子类的实例替换，不会影响程序正常运行。
    - 优点：更高的复用性
- 接口隔离原则
    - 多个专用接口比一个通用接口好
    - 优点：不必实现不需要的接口方法，接口方法专用；更易于重构
- 依赖倒置（Dnpendency Inversion Principle）
    - 指的是应该用抽象（接口）去替代高层次模块对低层次模块的依赖。
    - 优点：解耦合
    - 我倒觉得叫依赖转移更贴切，Dependency Transfer Principle，其实并没有真的倒转什么，叫依赖倒置反而让人觉得不太好理解。或者可以称之为面向接口编程。
- Thanks: [SOLID Principles : The Definitive Guide](https://android.jlelse.eu/solid-principles-the-definitive-guide-75e30a284dea)

## Language
- Java
    - [内存模型](https://blog.csdn.net/javazejian/article/details/72772461#comments)
    - https://juejin.im/post/5ab8d3d46fb9a028ca52f813
    - 原子性：https://www.jianshu.com/p/cf57726e77f2
    - 垃圾回收机制
        - 垃圾回收机制： 
            - 垃圾回收回收的堆内存；堆内存分为新生代和老生代，比例1：2； 对象是否应该被回收，涉及的算法主要是引用计数法和可达性分析算法。 
            - GC回收算法有： 标记清除法；分为标记阶段和清除阶段，先标记出需要回收的对象，再清除标记的对象所占用的内存。优点就是容易实现，缺点是容易产生内存碎片，下次申请大内存的实话可能会导致提前GC。

            - 复制算法；把内存按容量分为大小相等的两块,每次只使用一块。当其中一块内存用完了，把存活对象移动到一块中，回收之前的那一块内存。优点，运行高效，不容易产生内存碎片；缺点是内存空间使用等于缩小了一半，而且如果存活对象多的时候，效率会比较低。比较使用于存活对象少，回收对象多的场景。

            - 标记整理法；标记阶段和标记清除法一样，都是标记出存活对象和可回收对象；然后把存活对象都推到内存的一端，然后清楚掉端边界以外的内存区域。优点，适用于存活对象多，回收对象少的场景。

            - 分代回收算法（复制算法+标记整理法的优点结合） 我们根据对象存活的生命周期来采用不同的算法回收内存。一般堆内存分为新生代和老生代。新生代回收对象多，存活对象少，适合复制算法；老生代回收对象少，存活对象多，适合标记整理算法。

            - 垃圾回收的时候，为了准确性，所有引用的状态不能改变，程序执行处于暂停状态，这也是GC卡顿的由来，但是一般来说这种卡顿都很短暂，不容易察觉，如果内存管理出现问题，就会有卡顿严重的感觉。
- Rxjava
- Kotlin
- C
    - C入门记录
- C++
# The Basis os Android
## Activity 
- Lifecycle
    - onPause()轻量存储
    - onStop()重量回收，但是不宜太耗时
    - onDestroy()回收
- Launchmode
    - standard
    - 每次都创建
    - singleTop
    - 事例在栈顶，就复用，否则重新创建。
    - singleTask
    - 栈内存在实例就复用，但是会清空在事例之上的所有其他实例；不存在就创建
    - 不存在所属任务栈，先创建任务栈再创建事例，并且入栈
    - singleInstance
    - 只能独立的存在一个任务栈;如果实例存在直接提到栈顶，这时候会调用
    ```
    如果实例还没有被创建，就走正常的Activity正常周期；
    启动模式生效时，本身在栈顶，所走的生命周期如下：
    onPause（） 
    onNewIntent（） 
    onResume（）

    启动模式生效时，本身不在栈顶，所走的生命周期如下：
    onNewIntent() 
    onRestart() 
    onStart() 
    onResume() 

    ```
## Fragment
## Broadcast
    - 广播有哪些类型？
    - 本地广播的实现原理
    - EventBus 类的广播的实现
## View
    - View位置坐标由以ViewGroup的左上角为顶点的坐标系来决定的，向右是x轴正方形，向下是y轴
正方向；
    - View的触摸事件
    - MotionEvent
    - TouchSlop，被系统认为是最小的滑动距离，滑动距离必须大于等于这个值才会被系统认为是滑动事件。
    - ConstractLayout
    - [Appbarlayout](https://developer.android.com/reference/android/support/design/widget/AppBarLayout)
    - [CoordinatorLayout](https://developer.android.com/reference/androidx/coordinatorlayout/widget/CoordinatorLayout.html)
    - [NestedScrollView](https://developer.android.com/reference/android/support/v4/widget/NestedScrollView?hl=en)
### Android动画
- View Animation：作用在View对象上，
  - tween Animation:写出开始和结束状态，系统会补充中间过程。有translate,scale,rotate,alpha四种效果动画。
  - frame Animation：自定义动画的每一帧。
- Property Animation：可以不作用在View对象上（ValueAnimation），也可以作用在对象上(objectAnimation)，具有更高的可扩展性。是在API 11时提供等。
  - ValueAnimation
  - ObjectAnimation

  - 可以监听动画效果，也可以使用插值器来控制动画速度。
  - ![image](https://user-gold-cdn.xitu.io/2019/2/22/169158b7461a93ba?w=1140&h=270&f=png&s=75625)
  
- Thanks:
    - [Android动画总结](https://blog.csdn.net/carson_ho/article/details/79860980)
    - [interpolator&TypeEvaluator](https://blog.csdn.net/carson_ho/article/details/72863901)
- 事件传递
- [ConstraintLayout](https://mp.weixin.qq.com/s/JijR16p-DjlsZz8wn5D-PQ)(放一篇总结的很不错的文章)

## Data Store
- 存储的方式
- SharedPreference原理
- 如果想实现跨应用之间的数据操作，怎么实现？
- 如果需要跨进程读写呢？

- SharedPreferences
## 数据传递
- Parcelable
    - 不写set方法，数据传递失效.   
## Android消息机制
- Handler
    - 原理:ThreadLocal可以在每个线程中存储数据并且获取数据，要使用Handler，线程必须拥有Looper，当前一开始时没有Looper的，Looper被创建存储在ThreadLocal中。 消息被存储在MessageQueue这个单链表中，Looper无限的从队列中取消息来处理。ActivityThread就是UI线程，ActivityThread被创建的时候就初始化了Looper，这是UI线程默认可以使用Handler的原因。



## Executor(Interface),ThreadPoolExecutor(Impl)
- [java][java]
# Architecture
- MVP
- MVVM
- Dagger2
     - [Dagger2源码分析](https://github.com/BryceLee/android-compass/blob/master/FramesSourceAnalysis/Dagger2SourceAnalysis.md)
# JetPack
- Paging
    - [office document](https://developer.android.google.cn/topic/libraries/architecture/paging/#java)
    - [Introduce to Paging](https://mp.weixin.qq.com/s?__biz=MzIwMTAzMTMxMg==&mid=2649492903&idx=1&sn=6040b030d2a8125f38b7c9e7bd8f3054&chksm=8eec8658b99b0f4e07cf1c550096b87c5551a6da124906a67911dcae4a0656814a6e5cc13c84#rd)
    - [Add footer and header for paging](https://juejin.im/post/5caa0052f265da24ea7d3c2c#heading-5) ([Compare paging to BaseRecyclerViewAdapterHelper](#adapter))
- [Databinding][databinding]
<span id="adapter"></span>
# Powerful Open Source Project
- Adapter:
    - [BaseRecyclerViewAdapterHelper](https://github.com/CymChad/BaseRecyclerViewAdapterHelper)
- Utils:
    - [AndroidUtilCode](https://github.com/Blankj/AndroidUtilCode)
        - SpanUtils(TextView Rich Text Utils,Useful!)
        - 
        - ……
- Arouter
    - https://www.jianshu.com/p/857aea5b54a8 原来Arouter的跳转原理，就是用过注解处理器（annotationProcessor），扫描所有使用了@Route的类，把path信息和对应Class文件和一些参数绑定起来，并且并保存在相应的HaspMap里，path是key值，value是RouteMeta的对象。然后在跳转的时候再把数据拿出来使用。
    - 实际上还是用context.startActivity(inent)
- Eventbus
    - https://juejin.im/post/5ae2e6dcf265da0b9d77f28e EventBus使用了观察者模式。在运行时默认使用反射来找到订阅的方法，这种方式，在大量使用EventBus的情况下，会有效率问题；也可以在编译期间，通过注解处理器（annotationProcessor）来生成辅助类，保存订阅方法的相关信息，类似ButterKnife，Arouter的做法。
- ButterKnife
    - https://juejin.im/post/5acec2b46fb9a028c6761628 通过注解处理器，把生成类的特征信息保存在编译生成类里，通过JavaPoet来辅助生成Java类代码
    - ButterKnife.bind(this)-->docorview和对应的Class
    - docorview.findViewByid(R.id...)
## Network
- Okhttp
- Retrofit
- Volley
- Gson
- FastJson
## Useful Open Source Project
- [FlycoRoundView](https://github.com/H07000223/FlycoRoundView):replace drawable shape,(a lot of drawable .xml will make you crazy),



## Image Loader 
- Fresco
    - Fresco Architecture Learning：
        - https://juejin.im/post/5c7753a86fb9a049d61e37dd
- Glide
- Piccaso
- Universal-image-loader
- [If you want to compare them all,you can see this project](https://github.com/liaohuqiu/fresco-demo-for-gradle)

## Video Player
- [ExoPlayer](https://github.com/google/ExoPlayer)
- IjkPlayer

    - [apply() vs commit()](https://www.jianshu.com/p/3b2ac6201b33)
# 数据加密
# Optimize
- Layout
    - ViewStub
    - Inclide
    - Merge
    - Contractlayout
- Bitmap
- 内存泄露容易出现的场景
    - Context泄漏（被某个静态类引用，比如单例）
    - 匿名内部类
    - 类似Handler这样具备延时操作的场景
    - 无限循环Anime，没有关闭，就会导致Activity内存泄漏
    - 在服务（系统服务或者自定义服务）中注册监听器，必须及时取消监听
    - Rxjava配合RxLifecycle
     - 静态内部类+弱引用
- 方案：
    - Context可以用ApplicationContext代替,及时释放，解绑，解监听
## CPU
- [Inspect CPU](https://developer.android.com/studio/profile/cpu-profiler)
- 查看内存使用情况
    - adb shell dumpsys meminfo packagename
- https://juejin.im/entry/589542ed2f301e0069054007
https://www.jianshu.com/p/ac00e370f83d

- 卡顿查找工具
    - BlockCanary
        - [原理](https://www.jianshu.com/p/e58992439793)
        - 原理简述，Looper的loop（）会调用handler的dispatchMessage()；如果卡顿，一定是发生在dispatchMessage里，我们就在dispatchMessage（）执行之后设置监听，如果事件差大于16ms就认为是卡顿。dump堆栈和CPU信息就好。
    - Traceview
- 频繁GC（内存抖动）
    - 内存抖动(Memory Churn), 即大量的对象被创建又在短时间内马上被释放。
    - 瞬间产生大量的对象会严重占用 Young Generation 的内存区域, 当达到阀值, 剩余空间不够的时候, 也会触发 GC。即使每次分配的对象需要占用很少的内存，但是他们叠加在一起会增加 Heap 的压力, 从而触发更多的 GC
- 启动优化
    - [android official docs:performance/vitals/launch-time](https://developer.android.com/topic/performance/vitals/launch-time#profiling)
    - [Android App 冷启动优化方案](https://juejin.im/post/5aec28bb6fb9a07ac90d13dc)
    - [支付宝 App 启动速度优化](https://mp.weixin.qq.com/s/dATfVyGQRTQ9KV1L3RueUQ)(作者: 入弦 | 来源：公众号 mPaaS  )
        - 核心思路：对Dalvik做抑制GC回收，空间换时间；
- ANR

# Hybrid
- Fultter
- Rn
- Weex
- WebView
- links
    - [Flutter 要全平台制霸？我看悬](https://mp.weixin.qq.com/s/VnnhurJ03vEb75uB2PS6Wg)
## Route
- Aroute
    - @Route
    - @Servive
    - @Autowire
    - @Interceptor(AOP)

## Communication
- [Eventbus](https://github.com/greenrobot/EventBus)
- Rxbus

## JNI
- 生成so流程
    - 定义native方法
    - javah生成头文件
    - 写原生代码
    - Android.mk
    - Application.mk
    - 生成so，ndk-build或者cmake
- 引用
    - 必须写和so中一样的native方法同样的包名，类名，方法名
    - 配置gradle中设置ndk modulename 
    - System.loadLibrary("modulename");
## NDK
- NDK-Build

## Gralde
- Groovy
# Android Studio
- [adb#shellcommands](https://developer.android.com/studio/command-line/adb#shellcommands)
## App Version Update
- [Bugly](https://bugly.qq.com/docs/introduction/app-upgrade-introduction)
    - [Buyly Summary](https://www.jianshu.com/p/168feeea2363)

## Bult-in Test
You can upload your apk ,and you get a qrcode that someone can scan and download apk for test it.I think Pgyer is better because it can keep more valid apk history.
- [Pgyer](https://www.pgyer.com/)
- [Fir](https://fir.im/)
## Proguard
- Proguard
- read proguard code
    - proguardgui.sh(android_sdk_path/tools/proguard/bin)
        - 利用mapping还原被混淆的代码
        - Mac在terminal输入proguardgui.sh既可打开工具
## IM
- [Easemobe](http://www.easemob.com/product/im)（环信IM） 
    - [Customer Service](http://docs.easemob.com/cs/300visitoraccess/androidsdk)(客服云)

- [RongIM](https://www.rongcloud.cn/)(融云)

## Version Control Tools
- [Git](https://git-scm.com/book/en/v2)
- [SourceTree](https://www.sourcetreeapp.com/)(A Git UI Client)

## Example
- https://blog.csdn.net/singwhatiwanna/article/details/49560409
- https://juejin.im/post/5cc3a7495188252e784498b4

## Debug
- Charles
    - [Introduce to Charles](https://juejin.im/post/5b4f005ae51d45191c7e534a)
    - Map(When service don't give data,you can custom data)
- Python Request

## Bug Manage
- Bugly
- Bugtags

## team management
- Tower
- Trello
- [jira](https://freshservice.com/compare-it-helpdesks/jira-alternative?utm_source=Google-AdWords&utm_medium=Search-MEnA-UAE(Brand&Comp)&utm_campaign=Search-MEnA-UAE(Brand&Comp)&utm_term=jira&device=c&gclid=CjwKCAjw2cTmBRAVEiwA8YMgzbRLCdCyGwgMtOOp-i3glSCnsBSQGMlxHR5PZoBzlV9htwwFzq-X7xoCR20QAvD_BwE)
- ClearQuest
- [Zentao](https://www.zentao.net/)

## 反编译
- [Mac下反编译教程][http://www.devio.org/2018/05/08/Android-reverse-engineering-for-mac/]

## UI
- UI不用[蓝湖](https://lanhuapp.com)类设计稿管理工具，客户端就难受系列....
# 开发场景
## 音视频
## 电商场景
- Tangram
    - https://blog.csdn.net/u013541140/article/details/89517186
- vlayout
- 统计
    - [Umeng AppTrack](https://developer.umeng.com/docs/67964/detail/71107),(注意需要自行配置U-App的自定义事件，文档容易让人误解不需要配置）

[Algorithms]:https://github.com/BryceLee/algorithms-learning
[C_Primary]:https://github.com/BryceLee/algorithms-learning
[databinding]:https://github.com/BryceLee/android-compass/blob/master/jetpack/databinding.md
[java]:https://github.com/BryceLee/android-compass/blob/master/languages/java.md
# Thanks:
- 《Android开发艺术探索》
- [面试时究竟在问些什么](http://blog.zhaiyifan.cn/2019/01/25/when-i-talk-about-interview/)
- [Android APP 卡顿问题分析及解决方案](https://blog.csdn.net/zhanggang740/article/details/80199435)
