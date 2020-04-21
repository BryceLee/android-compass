# android-compass
Android-Compass is a learning manual about CS basis,Android basis,Android Architecture,useful open resource projects and some knowledge points of performance optimization. 
### [README OF ENGLISH](https://github.com/BryceLee/android-compass/blob/master/README_EN.md)
## [CS Basis](https://github.com/BryceLee/android-compass/blob/master/computerBasis/basis.md)
## [Android Basis][androidBasis]
## 程序设计相关
- [设计思想，设计原则，设计模式][design]
# Architecture
- MVC
- MVP
- MVVM
- [MVC,MVP,MVVM总结可以看这篇文章](https://tech.meituan.com/2016/11/11/android-mvvm.html)
- Clean
    ![](https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg)
    更适用Android的版本：
    ![](https://listenzz.github.io/images/android/Graph-1-2.png)
    - 基础特性:
        - 框架独立，如工具一般随时可拆卸替换使用。
        - 易测试，业务逻辑可以独立测试，不需要UI，数据等外部细节。
        - UI独立，UI修改完全不影响其他业务逻辑。
        - 数据独立，切换数据库不影响其他业务逻辑。
        - 外部代理独立，业务逻辑完全不依赖外部细节。
    - 并不是必须是四层结构，这里知识为了展示原理，只要遵循这些思想即可。
    - 从外向内的单向依赖。
    - 控制的流程是从外到里再到外的，注意上图右下角。（[对于这一点，如果有疑问你可以看这里](https://five.agency/wp-content/uploads/2016/11/Graph-1-2.png）
    - Use Cases的出现缓解了Presenter的压力   
        - 项目够大的情况下，会发现MVP有几个重大缺点
            - 新增一个V的状态就可能需要多写几个接口，P层接口会很臃肿
            - View的接口强依赖View的具体类型（变更View不方便）
    - 上图中的Presenter我们可以自行选用MVC，MVP，MVVM。
    - 关于UseCase的实践:
    ![](https://listenzz.github.io/images/android/graf_1.png)
    - 关于模块，见下图：
    ![](https://listenzz.github.io/images/android/Graph-2-2.png)
    - [参考文章][Uncle Bob's Clean]，[参考视频](https://www.youtube.com/watch?v=Nltqi7ODZTM)
    - 示范：
        - [MVP + Clean Architecture](https://github.com/android/architecture-samples/tree/todo-mvp-clean)
        - [Use Cases/Interactors in Domain layer](https://github.com/android/architecture-samples/tree/usecases)，就像这里所提到的一样，包括上面MVP + Clean Architecture，都是一个Clean的简化，由于项目比较小，不足以完整展示Clean架构，但是他们都遵循了Clean架构，我们参考即可。
        - 感谢以下作者，译者的付出：
            - [Android 架构：Part 4 —— 实践 Clean Architecture（含源码](https://listenzz.github.io/android-architecture-part-4-applying-clean-architecture-on-android-hands-on.html)等系列译文
            - [Android Architecture系列原文](http://five.agency/android-architecture-part-3-applying-clean-architecture-android/)
            - [Android-CleanArchitecture](https://github.com/android10/Android-CleanArchitecture)
            - https://juejin.im/post/59cace88f265da0648446780
            - https://juejin.im/post/5b87f3c9e51d45387e51dcf7
            - [Reedly](https://github.com/fiveagency/Reedly)
## 渲染机制
# JetPack
- Paging
    - [office document](https://developer.android.google.cn/topic/libraries/architecture/paging/#java)
    - [Introduce to Paging](https://mp.weixin.qq.com/s?__biz=MzIwMTAzMTMxMg==&mid=2649492903&idx=1&sn=6040b030d2a8125f38b7c9e7bd8f3054&chksm=8eec8658b99b0f4e07cf1c550096b87c5551a6da124906a67911dcae4a0656814a6e5cc13c84#rd)
    - [Add footer and header for paging](https://juejin.im/post/5caa0052f265da24ea7d3c2c#heading-5) ([Compare paging to BaseRecyclerViewAdapterHelper](#adapter))
- [Databinding][databinding]
<span id="adapter"></span>

# Powerful Open Source Project
## [javapoet](https://github.com/square/javapoet)
- A java library for generating .java source files.
- [可以看这篇文章](https://juejin.im/post/5bc96b63e51d450e4369ba19)
- 很多库都依赖了javapoet，比如ButterKnife，Arouter等等
## Adapter:
- [BaseRecyclerViewAdapterHelper](https://github.com/CymChad/BaseRecyclerViewAdapterHelper)
## [AndroidUtilCode](https://github.com/Blankj/AndroidUtilCode)
- SpanUtils(TextView Rich Text Utils,Useful!)
- ... 
## [Arouter](./sourceAnalysis/Arouter.md)
## [Eventbus（基于3.1.1源码）](./sourceAnalysis/Eventbus.md)
## [ButterKnife](./sourceAnalysis/ButterKnife.md)
## [Dagger2](./sourceAnalysis/Dagger2.md)
## [Rxjava](./sourceAnalysis/Rxjava.md)
## Network
### Okhttp
#### Intercrptors
Interceptors are a powerful mechanism that can monitor, rewrite, and retry calls.
OkHttp uses lists to track interceptors, and interceptors are called in order.
[更多关于Okhttp][okhttp]
### Retrofit
- 原理：用Java的动态代理，把定义的接口转换成具体的请求，并且得到请求结果。动态代理中，通过反射拿到方法的注解信息，构造Http请求。
- [多个BaseUrl，一个Retrofit实例](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2017/0726/8267.html)
- 这里我不想接入上面的库，但是[HostSelectionInterceptor.java
](https://gist.github.com/swankjesse/8571a8207a5815cca1fb)会影响全路径的请求，可以在上面的基础上改一下：
    - https://blog.csdn.net/jason_996/article/details/78659019
- Volley
- Gson
- FastJson

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
# Useful Open Source Project
## [FlycoRoundView](https://github.com/BryceLee/FlycoRoundView)
- replace drawable shape,(a lot of drawable .xml will make you crazy)
## [RxCache](https://github.com/BryceLee/RxCache)
- Reactive Caching library.
    
# 数据加密
- [EncriptSharedPreferences](https://developer.android.com/jetpack/androidx/releases/security):provides an implementation of SharedPreferences that automatically encrypts/decrypts all keys and values
# Dependency injection
- Another approach is to just use the normal Dagger APIs and follow guides such as the one 
[here](https://developer.android.com/training/dependency-injection/dagger-android). This may be simpler to understand but comes with the downside of having to write extra boilerplate manually
- The better approach is [here](https://dagger.dev/android) for Android.
# Optimize
## ANR
- common issues
        - UI线程5秒
        - BroadCastRecevier5秒
        - Service10秒
- dignose
- Strict Mode
            - Using StrictMode helps you find accidental I/O operations on the main thread while you’re developing your app. You can use StrictMode at the application or activity level
            - StrictMode is a developer tool which detects things you might be doing by accident and brings them to your attention so you can fix them.
- 生成.trace文件，进一步分析
- [Tips](https://developer.android.com/training/articles/perf-tips)
## UI优化
- View复用
    - View Cache Pool
- Layout更小的开销
    - ViewStub
    - Include
    - Merge
    - Contractlayout替代RelativeLayout,LinearLayout
- 不要重复设置背景
- 抛弃XML，用代码来写View（可以从telegram项目代码学习）
## 存储优化
### SharePreference
- 原理
- [apply() vs commit()](https://www.jianshu.com/p/3b2ac6201b33)
- MMKV替代SharedPreference
    - Writing random int for 1000 times, we get this chart:
    ![](https://github.com/Tencent/MMKV/wiki/assets/profile_android_mini.jpg)
    - [MMKV link](https://github.com/Tencent/MMKV)(api 16)
### JSON解析 
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
    - TraceView已经被废弃，从Android Studio3.2开始建议用CPU Profiler来替代。
    - Android Deviec Monitor在Android Studio3.1被废弃,在Android Studio3.2被移除.
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
    - Context泄漏（被某个静态类引用，比如单例）；解决：Context可以用ApplicationContext代替,及时释放，解绑，解监听。
    - 匿名内部类
    - 类似Handler这样具备延时操作的场景(1,静态内部类+弱引用;2.及时取消订阅)
    - 无限循环Anime，没有关闭，就会导致Activity内存泄漏
    - 在服务（系统服务或者自定义服务）中注册监听器，必须及时取消监听
    - Rxjava网络请求（可以配合RxLifecycle,[AutoDispose](https://github.com/uber/AutoDispose)） 
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
### [启动优化][app_launcher_optimize]
### [减少APK体积][reduce_apk_size]
### 线程优化
- [官方文档](https://developer.android.com/topic/performance/threads)
- Each thread costs a minimum of 64k of memory.
- Many system processes and third-party libraries often spin up their own threadpools. If your app can reuse an existing threadpool, this reuse may help performance by reducing contention for memory and processing resources.
https://juejin.im/post/5cebc989e51d454f72302482?utm_source=gold_browser_extension#heading-14
- 在 Application 的业务初始化代码加入进程判断，确保只在自己需要的进程初始化
#### [使用线程池](./optimize/threadpool.md)
#### [Coroutines](https://kotlinlang.org/docs/reference/coroutines/coroutines-guide.html)

# Hybrid
- Fultter
- Rn
- Weex
- WebView
    - Js&Native Interaction
        - JavaScriptInterface
            - js用这样的代码调用native:window.JavaScriptInterface.callHandler...
            - 使用@JavaScriptInterface来绑定交互方法
        - WebViewJavascriptBridge
            -  js用这样的代码调用native:window.WebViewJavascriptBridge.callHandler...
            - [WebViewJavascriptBridge](https://blog.csdn.net/sk719887916/article/details/47189607)
        - [WebView性能、体验分析与优化](https://tech.meituan.com/2017/06/09/webviewperf.html)
    - Local Debug
        - Put the js code under main/assets
        - load url:"file:///android_asset/yourfilename.html"
- links
    - [Flutter 要全平台制霸？我看悬](https://mp.weixin.qq.com/s/VnnhurJ03vEb75uB2PS6Wg)


# Apk
- 签名校验
    - jarsigner -certs -verbose -verify xxx.apk ([apk是否已经签名？](https://blog.csdn.net/qq_36005519/article/details/53519481))
# 多进程
## Common issues
- static variable and single instance failed
- thread syncronized failed
- SharedPreferences is not trusted(MMKV is trusted,it is Tencent open resouce project)
- Application will be crated multiple times. 
# 组件化
- [CC](https://github.com/luckybilly/CC)
- [Aroute](https://github.com/alibaba/ARouter)
    - @Route
    - @Servive
    - @Autowire
    - @Interceptor(AOP)
- [Dilutions](https://github.com/HomHomLin/Dilutions)
- [Andromeda](https://github.com/iqiyi/Andromeda)
# 插件化
# [Xposed](https://github.com/BryceLee/android-compass/blob/master/design.md)
# Android Studio
- [adb#shellcommands](https://developer.android.com/studio/command-line/adb#shellcommands)
### Old Version
- 可以去[这里](https://www.androiddevtools.cn/)下载旧版本
- Mac环境下：
    - 打开dmg文件，把AndroidStudio移动Applicaiton下，（正常安装布局，本地已经有As会弹出是否覆盖选项，选择keep both）
    - 默认是AndroidStudio 2，可以自行更名
    - 我这边选择不导入旧配置，不确定选择旧配置是否会有影响，所查资料也是建议用全新的配置，可以在隐藏文件~Library/Application Support看到新的As版本信息
# Gralde
- [Gradle和Plugin对应版本要求](https://developer.android.com/studio/releases/gradle-plugin.html#updating-gradle)
- Groovy
- [命令](https://blog.csdn.net/qq402164452/article/details/70207279)
### 配置编译
- 生成APK过程
![](https://developer.android.com/images/tools/studio/build-process_2x.png)
- 合并清单文件
![](https://developer.android.com/studio/images/build/manifest-merger_2x.png)
# App Version Update
- [Bugly](https://bugly.qq.com/docs/introduction/app-upgrade-introduction)
    - [Buyly Summary](https://www.jianshu.com/p/168feeea2363)
# Test
## Junit4(local unit test)
- Junit is the most popular and widely-used unit testing framework for java.
- a test method begins with the @Test annotation and contains the code to exercise and verify a single functionality in the component that you want to test.
- if you meet runtimeException--Error: "Method ... not mocked",you should add some configuration,because you run a test that calls an API from the Android SDK that you do not mock.
```
    android {
    // ...
        testOptions {
        unitTests.includeAndroidResources = true
        }
    }
```
## [robolectric](https://github.com/robolectric/robolectric)
- 上手robolectric填坑推荐阅读: [浅谈测试之
Robolectric](https://juejin.im/post/5a5c920e6fb9a01cba4290c9)
## Mockito
- [推荐阅读：浅谈测试之Mockito](https://juejin.im/post/5acc761ef265da237d035676)
## 其他测试资源
- 【Chinese Blog About Unit Test】（https://www.jianshu.com/p/aa51a3e007e2）
- dagger2结合单元测试,推荐阅读：[android-unit-testing-di-dagger](https://chriszou.com/2016/05/10/android-unit-testing-di-dagger.html)
- [Android单元测试(五)：网络接口测试](https://blog.csdn.net/qq_17766199/article/details/78881992)
## Built-in Test
- Auto Pack
    - [Jenkins](https://jenkins.io/doc/)
    - [Chinese Introduction](https://blog.csdn.net/ATangSir/article/details/71699403)
    - 注意点:
        - 关联Git仓库的时候要添加证书，方式一，仓库HTTP地址+仓库账号密码；方式二，本地创建jenkins SSH，jenins上配置私钥，仓库里配置共钥。[说明](https://www.cnblogs.com/liyuanhong/p/5762981.html)
        - 一定要在Global Tool Configuration（全局工具配置）里配置Git,Gradle信息，前者不配置，拉不到数据，后者不配置无法构建apk
- You can upload your apk ,and you get a qrcode that someone can scan and download apk for test it.I think Pgyer is better because it can keep more valid upload apk history.
- [Pgyer](https://www.pgyer.com/)
- [Fir](https://fir.im/)
# 混淆
## Proguard
- read proguard code
    - proguardgui.sh(android_sdk_path/tools/proguard/bin)
        - 利用Retrace功能，结合mapping还原被混淆的代码
        - Mac在terminal输入proguardgui.sh既可打开工具
## R8
# Version Control Tools
- [Git](https://git-scm.com/book/en/v2)
    - git commit --amend(修改未push的当前本地commit message)
- [SourceTree](https://www.sourcetreeapp.com/)(A Git UI Client)

# Debug
- Charles
    - [Introduce to Charles](https://juejin.im/post/5b4f005ae51d45191c7e534a)
    - Map(When service don't give data,you can custom data)
# Native
## JNI
- 目的
    - 较Java层而言，优化性能，对抗逆向
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
# Android 程序执行重要概念
## Dalvik
- Dalvik虚拟机，是Google等厂商合作开发的Android移动设备平台的核心组成部分之一。它可以支持已转换为.dex（即“Dalvik Executable”）格式的Java应用程序的运行。.dex格式是专为Dalvik设计的一种压缩格式，适合内存和处理器速度有限的系统。Dalvik由Dan Bornstein编写的，名字来源于他的祖先曾经居住过的小渔村达尔维克（Dalvík），位于冰岛埃亚峡湾。

- 大多数虚拟机包括JVM都是一种堆栈机器，而Dalvik虚拟机则是寄存器机。两种架构各有优劣，一般而言，基于堆栈的机器需要更多指令，而基于寄存器的机器指令更长。

从Android 5.0版起，Android Runtime（ART）取代Dalvik成为系统内默认虚拟机
## Android Runtime（缩写为ART）
- 是一种在Android操作系统上的运行环境，由Google公司研发，并在2013年作为Android 4.4系统中的一项测试功能正式对外发布，在Android 5.0及后续Android版本中作为正式的运行时库取代了以往的Dalvik虚拟机。ART能够把应用程序的字节码转换为机器码，是Android所使用的一种新的虚拟机。它与Dalvik的主要不同在于：Dalvik采用的是JIT技术，而ART采用Ahead-of-time（AOT）技术。ART同时也改善了性能、垃圾回收（Garbage Collection）、应用程序出错以及性能分析。（[From:wiki](https://zh.wikipedia.org/wiki/Android_Runtime)）

## bytecode
- 字节码（英语：Bytecode）通常指的是已经经过编译，但与特定机器代码无关，需要解释器转译后才能成为机器代码的中间代码。字节码通常不像源码一样可以让人阅读，而是编码后的数值常量、引用、指令等构成的序列。

- 字节码主要为了实现特定软件运行和软件环境、与硬件环境无关。字节码的实现方式是通过编译器和虚拟机。编译器将源码编译成字节码，特定平台上的虚拟机将字节码转译为可以直接运行的指令。字节码的典型应用为Java bytecode。
# Android Native Librarys
### Bionic
## Core Library

## Android 在内核中引入的特性
### Binder
- 负责进程间通信
- Android中各种服务都注册到Binder中
### ASHMem（匿名共享内存）
- 这是一种允许进程间共享内存的机制

# Team management
- Tower
- Trello
- [jira](https://freshservice.com/compare-it-helpdesks/jira-alternative?utm_source=Google-AdWords&utm_medium=Search-MEnA-UAE(Brand&Comp)&utm_campaign=Search-MEnA-UAE(Brand&Comp)&utm_term=jira&device=c&gclid=CjwKCAjw2cTmBRAVEiwA8YMgzbRLCdCyGwgMtOOp-i3glSCnsBSQGMlxHR5PZoBzlV9htwwFzq-X7xoCR20QAvD_BwE)
- ClearQuest
- [Zentao](https://www.zentao.net/)

# 逆向
- [Mac下反编译教程][http://www.devio.org/2018/05/08/Android-reverse-engineering-for-mac/]
# 榜样
- https://blog.csdn.net/singwhatiwanna/article/details/49560409
- https://juejin.im/post/5cc3a7495188252e784498b4

# 推荐书单
## Linux
- [性能之巅](https://book.douban.com/subject/26586598/)
## Android
- [最强Android书：架构大剖析](https://book.douban.com/subject/30269276/)
- 《Android开发艺术探索》
## 虚拟机
- [程序员的自我修养](https://book.douban.com/subject/3652388/)
- 垃圾回收算法手册
## 网络
- Web性能权威
## 操作系统
- Unix 网络编程
## 插件化
- Android插件化开发指南
## Opengl ES
- OpenGL ES 2 for Android
# Hook
- 翻译过来就是“钩子”，他的作用是截获进程对某个API的调用，使得执行流程指向我们期望的函数。
- GOT/PL Hook
    - GOT Hook
        - 微信的Matrix开源库ELF Hook（性能监控）
        - 爱奇艺开源的xHook
        - Facebook PLT Hook
- Trap Hook
- Inline Hook
# 架构

# Thanks:
- 《Android开发高手课》（张绍文）
- 《阿里巴巴 Android 开发手册》(1.0.0)
- 《最强Android书 架构大剖析》
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
[app_launcher_optimize]:./optimize/app_launcher_optimize.md

[okhttp]:https://github.com/BryceLee/android-compass/blob/master/FramesSourceAnalysis/Okhttp.md


[Algorithms]:https://github.com/BryceLee/algorithms-learning
[C_Primary]:https://github.com/BryceLee/algorithms-learning
[databinding]:https://github.com/BryceLee/android-compass/blob/master/jetpack/databinding.md
[java]:https://github.com/BryceLee/android-compass/blob/master/languages/java.md
[androidBasis]:https://github.com/BryceLee/android-compass/blob/master/androidBasis/androidBasis.md
[Basis]:https://github.com/BryceLee/android-compass/blob/master/computerBasis/basis.md
[Uncle Bob's Clean]:https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html
