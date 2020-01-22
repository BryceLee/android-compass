## Arouter
-  Arouter的跳转原理，就是用过注解处理器（annotationProcessor），扫描所有使用了@Route的类，把path信息和对应Class文件和一些参数绑定起来，并且并保存在相应的HashMap里，path是key值，value是RouteMeta的对象。然后在跳转的时候再把数据拿出来使用。
- 实际上还是用context.startActivity(inent)
- 推荐阅读https://www.jianshu.com/p/857aea5b54a8