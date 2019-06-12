![](https://developer.android.com/topic/performance/images/cold-launch.png)
## App launch can take place in one of three states:
- Cold Start
- At the beginning of a cold start, the system has three tasks. These tasks are:
    - Loading and launching the app.
    - Displaying a blank starting window for the app immediately after launch.
    - Creating the app process.
- Hot Start
    - A hot start of your application is much simpler and lower-overhead than a cold start. 
    - In a hot start, all the system does is bring your activity to the foreground.
    -  If all of your application’s activities are still resident in memory, then the app can avoid having to repeat object initialization, layout inflation, and rendering
    - However, if some memory has been purged in response to memory trimming events, such as onTrimMemory(), then those objects will need to be recreated in response to the hot start event.
    - A hot start displays the same on-screen behavior as a cold start scenario: The system process displays a blank screen until the app has finished rendering the activity.
- Warm Start 
    - A warm start encompasses some subset of the operations that take place during a cold start; at the same time, it represents less overhead than a hot start.
    - There are many potential states that could be considered warm starts. For instance:
        - The user backs out of your app, but then re-launches it. The process may have continued to run, but the app must recreate the activity from scratch via a call to onCreate().
        - The system evicts your app from memory, and then the user re-launches it. The process and the activity need to be restarted, but the task can benefit somewhat from the saved instance state bundle passed into onCreate().
## Diagnosing slow startup times
#### since Android4.4（API 19），we can filter logcat to know Activity startup time using keyword :Displayed.
- The elapsed time encompasses the following sequence of events:
    - Launch the process.
    - Initialize the objects.
    - Create and initialize the activity.
    - Inflate the layout.
    - Draw your application for the first time.
Activity的启动时间
```
Cold Start:
2019-06-11 14:09:59.456 1090-1154/? I/ActivityManager: Displayed packagename/.ui.home.activity.SplashActivity: +1s53ms
2019-06-11 14:10:02.519 1090-1154/? I/ActivityManager: Displayed packagename/.ui.home.activity.MainActivity: +225ms
Hot Start:
2019-06-11 14:11:23.501 1090-1154/? I/ActivityManager: Displayed packagename/.ui.home.activity.SplashActivity: +171ms
2019-06-11 14:11:26.555 1090-1154/? I/ActivityManager: Displayed packagename/.ui.home.activity.MainActivity: +166ms
```
#### You can also measure the time to initial display by running your app with the ADB Shell Activity Manager command. Here's an example:
```
adb shell am start -S -W com.example.app/.MainActivity
```
com.example.app is your package name,but the dot in front of MainActivity should be retained 。
```
$ adb shell am start -W packagename/.ui.home.activity.SplashActivity
Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] cmp=packagename/.ui.home.activity.SplashActivity }
Status: ok
Activity: packagename/.ui.home.activity.SplashActivity
ThisTime: 1483
TotalTime: 1483
WaitTime: 1516
Complete

```
## common issues
- Heavy app initialization
    - Reduce unnecessary initicalizations when the startup
- Any global singleton objects your app initializes.
    - Reduce global static objects,Consider using a dependency injection like dagger2 that created when used first time.
- Any disk I/O, deserialization, or tight loops that might be occurring during the bottleneck.
    - Reduce those operations.
- Heavy activity initialization
Activity creation often entails a lot of high-overhead work. Often, there are opportunities to optimize this work to achieve performance improvements. Such common issues include:
    - Inflating large or complex layouts.
        - The larger your view hierarchy, the more time the app takes to inflate it. Two steps you can take to address this issue are:
            - Flattening your view hierarchy by reducing redundant or nested layouts.
            - Not inflating parts of the UI that do not need to be visible during launch. Instead, use use a ViewStub object as a placeholder for sub-hierarchies that the app can inflate at a more appropriate time.
    - Blocking screen drawing on disk, or network I/O.
    - Loading and decoding bitmaps.
    - Rasterizing VectorDrawable objects.
    - Initialization of other subsystems of the activity.
    - Having all of your resource initialization on the main thread can also slow down startup. You can address this issue as follows:
        - Move all resource initialization so that the app can perform it lazily on a different thread.
        - Allow the app to load and display your views, and then later update visual properties that are dependent on bitmaps and other resources.
## Themed launch screens
- You can use the activity's windowBackground theme attribute to provide a simple custom drawable for the starting activity.
- For example, you might create a new drawable file and reference it from the layout XML and app manifest file as follows:

Layout XML file:
```
<layer-list xmlns:android="http://schemas.android.com/apk/res/android" android:opacity="opaque">
  <!-- The background color, preferably the same as your normal theme -->
  <item android:drawable="@android:color/white"/>
  <!-- Your product logo - 144dp color version of your app icon -->
  <item>
    <bitmap
      android:src="@drawable/product_logo_144dp"
      android:gravity="center"/>
  </item>
</layer-list>
```
```
<activity ...
android:theme="@style/AppTheme.Launcher" />
```
- The easiest way to transition back to your normal theme is to call setTheme(R.style.AppTheme) before calling super.onCreate() and setContentView():
## Application onCrete()
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
## [Generate Trace Logs by Instrumenting Your App](https://developer.android.com/studio/profile/generate-trace-logs)
# Thanks
- // Starts recording a trace log with the name you provide. For example, the
// following code tells the system to start recording a .trace file to the
// device with the name "sample.trace".
Debug.startMethodTracing("sample");
...
// The system begins buffering the generated trace data, until your
// application calls <code><a href="/reference/android/os/Debug.html#stopMethodTracing()">stopMethodTracing()</a></code>, at which time it writes
// the buffered data to the output file.
Debug.stopMethodTracing();
- Use Android Profiler analyze .trace file.
- [android official docs:performance/vitals/launch-time](https://developer.android.com/topic/performance/vitals/launch-time#profiling)
- [Android App 冷启动优化方案](https://juejin.im/post/5aec28bb6fb9a07ac90d13dc)
- [支付宝 App 启动速度优化](https://mp.weixin.qq.com/s/dATfVyGQRTQ9KV1L3RueUQ)(作者: 入弦 | 来源：公众号 mPaaS  )
        - 核心思路：对Dalvik做抑制GC回收，空间换时间；
- ANR
- Android GC(和JavaGC有一些区别)

