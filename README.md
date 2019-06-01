# android-compass
android-compass is a dev manual about Android Architecture,Third Libs ,Utils and Some Solutions to dev.
# [README OF ENGLISH](https://github.com/BryceLee/android-compass/blob/master/README_EN.md)

## The Basis of Computer
- 操作系统
    - 多进程
    - 多线程
- 算法和数据结构 
    - [Algorithms]
    - [HaspMap](https://github.com/BryceLee/algorithms-learning/blob/master/data-structure/java/hashmap.md)
    - LinkedList
    - 快排
    - 最大堆
    - 二分法
- [计算机网络相关][networkProtocol]
   

## 程序设计相关
- [设计思想，设计原则，设计模式][design]

## Language
- Java
    - [内存模型](https://blog.csdn.net/javazejian/article/details/72772461#comments)
    - https://juejin.im/post/5ab8d3d46fb9a028ca52f813
    - 原子性：https://www.jianshu.com/p/cf57726e77f2
    - [垃圾回收机制](https://github.com/BryceLee/java-compass/blob/master/README.md#heading-2)
           
- Rxjava
- Kotlin
- C
    - C入门记录
- C++
## Executor(Interface),ThreadPoolExecutor(Impl)
- 构造参数说明：
    - coolPoolsize
        - 默认情况下一直活着，除非设置ThreadPoolExecutor的allowCoreThreadTimeout=true,核心线程闲置超时也会被终止
    - maxinumPoolsize
        - 最大线程数，新任务超过最大值就要等待
    - keepAliveTime
        - 线程闲置保活时间
    - unit
        - keepAliveTime的单位
    - workQueue:任务队列，存储runnable对象
    - ThreadFactory  
        - 创建线程
- 参考AsyncTask参数配置：
    - coolposize=cpucount+1
    - maximumpoolsize=cpucount*2+1
    - 核心线程无超时机制，非核心线程超时事件1秒
    - 任务队列容量128
- 分类
        - FixedThreadPool
            - Executors.newFixedThreadPool()
            - 固定核心线程，全是核心线程，无超时机制，无任务队列上线 
            - 适合需要快速响应的任务
        - Executors.newCachedThreadPool()
            - 全是非核心线程，最大线程数Interget.Max_Value,60秒超时机制
            - 无法存储任务，有新任务立即执行
            - 适合执行大量耗时较少的任务（Retrofit?Rxjava?）
        - Executors.newScheduledThreadPool()
            - 核心线程固定，最大线程Inter.Max_Value
            - 超时时间为0，非核心线程闲置会被立即回收
            - 适合定时任务，具有周期性的任务
        - Executor.SingleThreadExecutor()
            - 只有一个核心线程
            - 单线程的任务
## Android Studio
- 可以去https://www.androiddevtools.cn/下载旧版本
- Mac环境下：
    - 打开dmg文件，把AndroidStudio移动Applicaiton下，（正常安装布局，本地已经有As会弹出是否覆盖选项，选择keep both）
    - 默认是AndroidStudio 2，可以自行更名
    - 我这边选择不导入旧配置，不确定选择旧配置是否会有影响，所查资料也是建议用全新的配置，可以在隐藏文件~Library/Application Support看到新的As版本信息
### keymap in common use
- Back
    - 返回上一个编辑的地方
- Move Caret to Line Start（把光标移动到行首）
- Move Caret to Line End(把光标移动到行尾)
## Gralde
- [Gradle和Plugin对应版本要求](https://developer.android.com/studio/releases/gradle-plugin.html#updating-gradle)

- Groovy
- [命令](https://blog.csdn.net/qq402164452/article/details/70207279)
# Android Basis
## Android DataStructure
- SpareArray
## View 
- [View的事件分发](https://blog.csdn.net/u010983881/article/details/78905598)
- ![](https://itimetraveler.github.io/gallery/android-view/1520093523-0.png)
## Android动画
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
## 缓存
- [Lrucache](https://developer.android.com/reference/android/util/LruCache)（Lru算法）
## 数据传递
- Parcelable
    - 不写set方法，数据传递失效.   
## Framework
## Activity 
- Lifecycle
    - onPause()轻量存储
    - onStop()重量回收，但是不宜太耗时
    - onDestroy()回收
- android:exported
    - 设置Activity是否能够被其他应用的组建唤起，”true“为允许，“false”为不允许
    - 如果你设置了"intent filter"就不应该设置exported=false，否则会抛出ActivityNotFoundException
    - 默认是false
    - 另一个属性也可以显示Activity被外部实体访问：permission
- android:hardwareAccelerated
    - Activity是否开启硬件加速渲染
- android:launchMode
    - Launchmode
        - standard
            - 每次都创建
        - singleTop
            - 实例在栈顶，就复用，否则重新创建。
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
## Service
    - AMS
    - PMS
## Handler
- 原理:ThreadLocal可以在每个线程中存储数据并且获取数据，要使用Handler，线程必须拥有Looper，当前一开始时没有Looper的，Looper被创建存储在ThreadLocal中。 消息被存储在MessageQueue这个单链表中，Looper无限的从队列中取消息来处理。ActivityThread就是UI线程，ActivityThread被创建的时候就初始化了Looper，这是UI线程默认可以使用Handler的原因。
    - Handler为什么会持有外部的引用：
        - Message会持有Handler的 引用，由于Java的特性，内部类会持有外部类的引用，使得Activity会被Handler持有，这样就可能导致Activity泄露。
        - Handler sendMessage()方法会调用enqueueMeaage(..)

    ```
    private boolean enqueueMessage(MessageQueue queue, Message msg, long uptimeMillis) {
        msg.target = this;//msg内部持有了Handler的引用
        if (mAsynchronous) {
            msg.setAsynchronous(true);
        }
        return queue.enqueueMessage(msg, uptimeMillis);
    }
        ```
## Permissions
- [overview](https://developer.android.com/guide/topics/permissions/overview)
## Binder
### View
- View位置坐标由以ViewGroup的左上角为顶点的坐标系来决定的，向右是x轴正方形，向下是y轴
正方向；
- View的触摸事件
- MotionEvent
- TouchSlop，被系统认为是最小的滑动距离，滑动距离必须大于等于这个值才会被系统认为是滑动事件。
- ConstractLayout
- [Appbarlayout](https://developer.android.com/reference/android/support/design/widget/AppBarLayout)
- [CoordinatorLayout](https://developer.android.com/reference/androidx/coordinatorlayout/widget/CoordinatorLayout.html)
- [NestedScrollView](https://developer.android.com/reference/android/support/v4/widget/NestedScrollView?hl=en)
## 渲染机制

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
    - [多个BaseUrl，一个Retrofit实例](https://www.jianshu.com/p/2919bdb8d09a)
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
    
# 数据加密
- [EncriptSharedPreferences](https://developer.android.com/jetpack/androidx/releases/security):provides an implementation of SharedPreferences that automatically encrypts/decrypts all keys and values
# Optimize
- ANR
    - UI线程5秒
    - Service10秒
- Tips
    - [Tips](https://developer.android.com/training/articles/perf-tips)
## UI优化
- View复用
    - View Cache Pool
- Layout更小的开销
    - ViewStub
    - Inclide
    - Merge
    - Contractlayout替代RelativeLayout,LinearLayout
- 不要重复设置背景
## 存储优化
- SharePreference
    - 原理
    - [apply() vs commit()](https://www.jianshu.com/p/3b2ac6201b33)
    - MMKV替代SharedPreference
        - Writing random int for 1000 times, we get this chart:
        ![](https://github.com/Tencent/MMKV/wiki/assets/profile_android_mini.jpg)
        - [MMKV link](https://github.com/Tencent/MMKV)(api 16)
- JSON解析 
    - ANDROID JSON
    - Gson
    - FastJson （最快）
    - MSON
- 序列化
    - Serializable
    - Parcelable
    - Seria
## 卡顿优化
- [Slow rendering](https://developer.android.com/topic/performance/vitals/render#top_of_page)
### 卡顿查找工具
- BlockCanary
    - [原理](https://www.jianshu.com/p/e58992439793)
    - 原理简述，Looper的loop（）会调用handler的dispatchMessage()；如果卡顿，一定是发生在dispatchMessage里，我们就在dispatchMessage（）执行之后设置监听，如果事件差大于16ms就认为是卡顿。dump堆栈和CPU信息就好。
    - 如果BlockCanary日志的信息还是无法分析出原因，建议监听CPU活动情况，比如Profiler的Trace Java Methods,然后可以看Flame Chart，找到和项目代码相关的函数，查看耗时，分析可疑函数即可。
- Linux命令
- 图形化工具
    - 实现方式有两个流派，instrument：获取某段时间内所有函数的调用的过程，分析函数调用过程，再进一步分析待优化的点；sample：有选择性或者用采样点方式观察某些函数的调用过程，推进出流程的可疑点，再细化分析；
    - instrument:
        - Traceview,性能损耗比较大
        - Naoscope(Uber)，性能损耗比较小
    - Sample:
        - systrace
    - Android Studio Profiler(降低开发门槛)
        - Sample Java Methods(类似traceview的sample类型)
        - Trace Java Methods(类似traceview的instrument类型)
        - Trace System Calls(类似systrace)
        - Sample C/C++ Calls（类似Simpleperf）
        - 以上分析工具都支持Call Chart和Flame Chart
            - Call Chart
                - Traceview,systrace默认用Call Chart来展示。它按照程序的函数执行数据来展示，适合分析整个流程的调用。像心电图。
            - 系统API显示橘色，自己的程序函数显示绿色，第三方函数（包括Java API）被显示为蓝色。
                ![](https://developer.android.com/studio/images/profile/call_chart_1-2X.png)
            - Flame Chart就是火焰图，以一个全局的视野来看待一段时间的调用分布。像X光。时间+空间的展示方式，从下而上是调用顺序，方法图形块的宽度代表时间比例。（可以做内存，I/O分析）。
### CPU
- 卡顿原因很多，但最终都会反应到CPU时间上：用户时间和系统时间，分别是用户应用程序代码执行消耗时间和执行内核态系统调用消耗时间，后者包括I/O，锁等
- [Inspect CPU](https://developer.android.com/studio/profile/cpu-profiler) 
- 频繁GC（内存抖动）
    - 内存抖动(Memory Churn), 即大量的对象被创建又在短时间内马上被释放。
    - 瞬间产生大量的对象会严重占用 Young Generation 的内存区域, 当达到阀值, 剩余空间不够的时候, 也会触发 GC。即使每次分配的对象需要占用很少的内存，但是他们叠加在一起会增加 Heap 的压力, 从而触发更多的 GC
- Android Profiler
    - https://blog.csdn.net/niubitianping/article/details/72617864
### 内存优化
- Bitmap
    - 8888
    - 565
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
- 查看内存使用情况
    - adb shell dumpsys meminfo packagename
- https://juejin.im/entry/589542ed2f301e0069054007
https://www.jianshu.com/p/ac00e370f83d
- [offical docs](https://developer.android.com/studio/profile/memory-profiler?hl=zh-cn)
- [leakcanary](https://github.com/square/leakcanary)
    - 1.使用弱引用监听Acitvity或者Fragmmnt的销毁，也可以自己手动调用API去监控一个View
    - 2.如果弱引用没有被消除，5秒后运行GC，如果实例还没被消除就是潜在的泄漏
    - 3.当潜在的泄漏数量达到阀值，就dumps Javaheap的引用信息到.hprof文件中，app显示的时候阀值是5，否则是1
    - 4.LeakCanary会找到一个实例的引用链，从实例到GCROOT最近的强引用一般是原因，但是还是得看情况分析
### 启动优化
- [android official docs:performance/vitals/launch-time](https://developer.android.com/topic/performance/vitals/launch-time#profiling)
- [Android App 冷启动优化方案](https://juejin.im/post/5aec28bb6fb9a07ac90d13dc)
- [支付宝 App 启动速度优化](https://mp.weixin.qq.com/s/dATfVyGQRTQ9KV1L3RueUQ)(作者: 入弦 | 来源：公众号 mPaaS  )
        - 核心思路：对Dalvik做抑制GC回收，空间换时间；
- ANR
- Android GC(和JavaGC有一些区别)
- [空闲队列加载](https://blog.csdn.net/tencent_bugly/article/details/78395717)
```
Looper.myQueue().addIdleHandler(new MessageQueue.IdleHandler() {
            @Override
            public boolean queueIdle() {
               //do something
                return false;
            }
        });
```
### [减少APK体积][reduce_apk_size]

https://juejin.im/post/5cebc989e51d454f72302482?utm_source=gold_browser_extension#heading-14
# Hybrid
- Fultter
- Rn
- Weex
- WebView
    - JavaScriptInterface
        - js用这样的代码调用native:window.JavaScriptInterface.callHandler...
            - 使用@JavaScriptInterface来绑定交互方法
        -  js用这样的代码调用native:window.WebViewJavascriptBridge.callHandler...
            - [WebViewJavascriptBridge](https://blog.csdn.net/sk719887916/article/details/47189607)
        - [WebView性能、体验分析与优化](https://tech.meituan.com/2017/06/09/webviewperf.html)
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
- 原理：
- Rxbus
## Apk
- 签名校验
    - jarsigner -certs -verbose -verify xxx.apk ([apk是否已经签名？](https://blog.csdn.net/qq_36005519/article/details/53519481))

        
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
- CMake
# 程序执行重要概念
## Android Runtime（缩写为ART）
- 是一种在Android操作系统上的运行环境，由Google公司研发，并在2013年作为Android 4.4系统中的一项测试功能正式对外发布，在Android 5.0及后续Android版本中作为正式的运行时库取代了以往的Dalvik虚拟机。ART能够把应用程序的字节码转换为机器码，是Android所使用的一种新的虚拟机。它与Dalvik的主要不同在于：Dalvik采用的是JIT技术，而ART采用Ahead-of-time（AOT）技术。ART同时也改善了性能、垃圾回收（Garbage Collection）、应用程序出错以及性能分析。（[From:wiki](https://zh.wikipedia.org/wiki/Android_Runtime)）
## Dalvik
- Dalvik虚拟机，是Google等厂商合作开发的Android移动设备平台的核心组成部分之一。它可以支持已转换为.dex（即“Dalvik Executable”）格式的Java应用程序的运行。.dex格式是专为Dalvik设计的一种压缩格式，适合内存和处理器速度有限的系统。Dalvik由Dan Bornstein编写的，名字来源于他的祖先曾经居住过的小渔村达尔维克（Dalvík），位于冰岛埃亚峡湾。

- 大多数虚拟机包括JVM都是一种堆栈机器，而Dalvik虚拟机则是寄存器机。两种架构各有优劣，一般而言，基于堆栈的机器需要更多指令，而基于寄存器的机器指令更长。

从Android 5.0版起，Android Runtime（ART）取代Dalvik成为系统内默认虚拟机
## bytecode
- 字节码（英语：Bytecode）通常指的是已经经过编译，但与特定机器代码无关，需要解释器转译后才能成为机器代码的中间代码。字节码通常不像源码一样可以让人阅读，而是编码后的数值常量、引用、指令等构成的序列。

- 字节码主要为了实现特定软件运行和软件环境、与硬件环境无关。字节码的实现方式是通过编译器和虚拟机。编译器将源码编译成字节码，特定平台上的虚拟机将字节码转译为可以直接运行的指令。字节码的典型应用为Java bytecode。
# Android Studio
- [adb#shellcommands](https://developer.android.com/studio/command-line/adb#shellcommands)
## App Version Update
- [Bugly](https://bugly.qq.com/docs/introduction/app-upgrade-introduction)
    - [Buyly Summary](https://www.jianshu.com/p/168feeea2363)

## Bult-in Test
- Auto Pack
    - [Jenkins](https://jenkins.io/doc/)
    - [Chinese Introduction](https://blog.csdn.net/ATangSir/article/details/71699403)
    - 注意点:
        - 关联Git仓库的时候要添加证书，方式一，仓库HTTP地址+仓库账号密码；方式二，本地创建jenkins SSH，jenins上配置私钥，仓库里配置共钥。[说明](https://www.cnblogs.com/liyuanhong/p/5762981.html)
        - 一定要在Global Tool Configuration（全局工具配置）里配置Git,Gradle信息，前者不配置，拉不到数据，后者不配置无法构建apk
You can upload your apk ,and you get a qrcode that someone can scan and download apk for test it.I think Pgyer is better because it can keep more valid apk history.
- [Pgyer](https://www.pgyer.com/)
- [Fir](https://fir.im/)
## Proguard
- Proguard
- read proguard code
    - proguardgui.sh(android_sdk_path/tools/proguard/bin)
        - 利用Retrace功能，结合mapping还原被混淆的代码
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
## update
- Bugly
    - update    
    - hot fix
        - 注意：Tinker不支持修改AndroidManifest.xml，Tinker不支持新增四大组件(1.9.0支持新增非export的Activity)；
        - [流行框架比较，原理说明](https://www.cnblogs.com/popfisher/p/8543973.html)
## Bug Manage
- Bugly
    - 必须设置
- Bugtags

## Team management
- Tower
- Trello
- [jira](https://freshservice.com/compare-it-helpdesks/jira-alternative?utm_source=Google-AdWords&utm_medium=Search-MEnA-UAE(Brand&Comp)&utm_campaign=Search-MEnA-UAE(Brand&Comp)&utm_term=jira&device=c&gclid=CjwKCAjw2cTmBRAVEiwA8YMgzbRLCdCyGwgMtOOp-i3glSCnsBSQGMlxHR5PZoBzlV9htwwFzq-X7xoCR20QAvD_BwE)
- ClearQuest
- [Zentao](https://www.zentao.net/)

## 反编译
- [Mac下反编译教程][http://www.devio.org/2018/05/08/Android-reverse-engineering-for-mac/]

## UI Desgin
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
# 推荐书单
## Linux
- [性能之巅](https://book.douban.com/subject/26586598/)
## Android
- [最强Android书：架构大剖析](https://book.douban.com/subject/30269276/)
- 《Android开发艺术探索》
## 虚拟机
- [程序员的自我修养]（https://book.douban.com/subject/3652388/）
- 垃圾回收算法手册
## 网络
- Web性能权威
## 操作系统
- Unix 网络编程
## Opengl ES
- OpenGL ES 2 for Android
# Thanks:
- 《Android开发高手课》（张绍文）
- 《Android开发艺术探索》
- [面试时究竟在问些什么](http://blog.zhaiyifan.cn/2019/01/25/when-i-talk-about-interview/)
- [Android APP 卡顿问题分析及解决方案](https://blog.csdn.net/zhanggang740/article/details/80199435)
- [CS-Notes](https://github.com/CyC2018/CS-Notes/blob/master/notes/%E8%AE%A1%E7%AE%97%E6%9C%BA%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%20-%20%E5%86%85%E5%AD%98%E7%AE%A1%E7%90%86.md#1-%E6%9C%80%E4%BD%B3)
- 《Android工程师的“面试指南”》（孙鹏飞）
- [Linux环境下进程的CPU占用率](http://www.samirchen.com/linux-cpu-performance/)
- [Android 中高级工程师面试复习大纲](https://juejin.im/post/5cdd7a94f265da03775c781a)
- [RFC wiki](https://zh.wikipedia.org/wiki/RFC)
- [Dalvik wiki](https://zh.wikipedia.org/wiki/Dalvik%E8%99%9A%E6%8B%9F%E6%9C%BA)

[design]:https://github.com/BryceLee/android-compass/blob/master/design.md
[networkProtocol]:https://github.com/BryceLee/android-compass/blob/master/networkProtocol.md
[reduce_apk_size]:https://github.com/BryceLee/android-compass/blob/master/optimize/reduce_apk_size.md