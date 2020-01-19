### View
- [View的事件分发](https://blog.csdn.net/u010983881/article/details/78905598)
- ![](https://itimetraveler.github.io/gallery/android-view/1520093523-0.png)
- View位置坐标由以ViewGroup的左上角为顶点的坐标系来决定的，向右是x轴正方形，向下是y轴
正方向；
- View的触摸事件
- MotionEvent
- TouchSlop，被系统认为是最小的滑动距离，滑动距离必须大于等于这个值才会被系统认为是滑动事件。
- ConstractLayout
- [Appbarlayout](https://developer.android.com/reference/android/support/design/widget/AppBarLayout)
- [CoordinatorLayout](https://developer.android.com/reference/androidx/coordinatorlayout/widget/CoordinatorLayout.html)
- [NestedScrollView](https://developer.android.com/reference/android/support/v4/widget/NestedScrollView?hl=en)
- [ConstraintLayout](https://mp.weixin.qq.com/s/JijR16p-DjlsZz8wn5D-PQ)(放一篇总结的很不错的文章)

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

## Handler
- 原理:
    - 应用启动的时候调用ActivityThread的main方法，Looper.prepareMainLooper被调用，初始化了主Looper，这也是UI线程默认可以使用Handler的原因。ThreadLocal可以在每个线程中存储数据并且获取数据，要使用Handler，线程必须拥有Looper，否则在执行Looper.loop()的时候将报错“"No Looper; Looper.prepare() wasn't called on this thread."。Looper被存储在对应的ThreadLocal中。 消息被存储在MessageQueue这个单链表中，调用Looper.loop(),Looper无限的从队列中取消息来交给Handler处理(MessageQueue.next()得到要处理的消息)。
    - Handler为什么会持有外部的引用：
        - 由于Java的特性，内部类会持有外部类的引用，使得Activity会被Handler持有，
        而Handler被Message持有，Message被MessageQueue持有，如果没有及时清掉消息，可能导致Activity泄露。消息如果正常队列中会移出消息，讲执行msg.target.dispatchMessage(msg)，消息处理完后会被回收，也将不再持有Handler的引用。
        - Handler sendMessage()方法会调用enqueueMeaage(..)

    ```
    //查看源代码也可以发现，我们常用的Handler.post(Runnable r)也是最后调用如下方法
    private boolean enqueueMessage(MessageQueue queue, Message msg, long uptimeMillis) {
        msg.target = this;//msg内部持有了Handler的引用
        if (mAsynchronous) {
            msg.setAsynchronous(true);
        }
        return queue.enqueueMessage(msg, uptimeMillis);
    }
        ```
    - 在ActivityThread调用完Looper.prepareMainLooper后，还将调用Looper.loop();
    - [推荐阅读](https://blog.csdn.net/will_ls/article/details/79040890)
## 绘制View辅助资源
- 根据View的状态来改变颜色
    - 在代码中，用ColorStateList来表示。
    - 在XML中，使用selector文件，位于res/color/filename.xml，示例：
    ```
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="true"
          android:color="#ffff0000"/> <!-- pressed -->
    <item android:state_focused="true"
          android:color="#ff0000ff"/> <!-- focused -->
    <item android:color="#ff000000"/> <!-- default -->
    </selector>
    ```
    [（点击查看详细描述）](https://developer.android.com/guide/topics/resources/color-list-resource)
## 数据传递
- Parcelable
- Serializable 
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
## Android DataStructure
- SparseArray
    - Better Performance
        - Clone
        - binarySearch
        - Gc

## Data Store
- 存储的方式
- SharedPreference原理
- 如果想实现跨应用之间的数据操作，怎么实现？
- 如果需要跨进程读写呢？
- SharedPreferences
## 缓存
- [Lrucache](https://developer.android.com/reference/android/util/LruCache)（Lru算法）
#### [ActivityLifecycleCallbacks]((https://www.jianshu.com/p/75a5c24174b2))
## Fragment
## Broadcast
(TODO)
- 广播有哪些类型
- 本地广播的实现原理
- EventBus 类的广播的实现
## Service
- ActivityManagerService(AMS)
    - AMS是Android中最核心的服务，主要负责系统中四大组件的启动、切换、调度及应用进程的管理和调度等工作，其职责与操作系统中的进程管理和调度模块相类似，因此它在Android中非常重要。
- PackageManagerService
    - PMS用来管理所有的package信息，包括安装、卸载、更新以及解析AndroidManifest.xml以组织相应的数据结构，这些数据结构将会被PackageManagerService、AMS等service和application使用.
## Communication
- Intent
- Bundle
- Serializable
- Parcelable
- Binder
- [AIDL](https://developer.android.com/guide/components/aidl?hl=zh-cn)
- ContentProvider
- Socket
- [Eventbus](https://github.com/greenrobot/EventBus)
- Rxbus
- LiveData
### Support IPC Communication
- Bundle
- A process Use Intent Start B process Service..And The Servie do something..
- Parcelable
- Binder
- [AIDL](https://developer.android.com/guide/components/aidl?hl=zh-cn)
- ContentProvider
- Messenger
### IPC potential risk
- 单例失效
- SharedPreferences不安全（因为内存中会有一份对应的缓存）
## Permissions
- [overview](https://developer.android.com/guide/topics/permissions/overview)
