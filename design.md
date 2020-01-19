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
    - 指的是对象被其子类替换，不会影响程序正常运行。
    - 优点：更高的复用性
- 接口隔离原则
    - 多个专用接口比一个通用接口好
    - 优点：不必实现不需要的接口方法，接口方法专用；更易于重构
- 依赖倒置（Dnpendency Inversion Principle）
    - 指的是应该用抽象（接口）去替代高层次模块对低层次模块的依赖。
    - 优点：解耦合
    - 我倒觉得叫依赖转移更贴切，Dependency Transfer Principle，其实并没有真的倒转什么，叫依赖倒置反而让人觉得不太好理解。或者可以称之为面向接口编程。
- 迪米特法则
    - 一个类对自己依赖的类知道的越少越好
    - 优点：解耦合。
- Thanks: [SOLID Principles : The Definitive Guide](https://android.jlelse.eu/solid-principles-the-definitive-guide-75e30a284dea)
## 设计模式
###  创建型：
- 单例
    - 做好线程同步，确保线程安全。不适用于多进程。
    - [参考链接](https://coolshell.cn/articles/265.html)
- 建造者
    - 用内部静态类来构造对象和参数配置。不用在构造方法加大量构造参数，比起set方法也更清晰优雅。对构造过程做到了更精细的控制。
- 工厂方法
    - 解决简单工厂带来的问题，简单工厂是一个工厂生产多个产品，违反了开闭原则，即新增产品就得修改工厂的实现代码。
    - 工厂方法实现思路：定义产品的抽象接口，把产品的实例化工作交给具体产品子类。定义工厂方法返回产品对象，具体工厂返回具体产品对象。一个工厂生产一个产品。
    - 新的问题：新增产品就得新增工厂。
- 抽象工厂
    - 定义抽象工厂，抽象产品，具体工厂，具体产品，一般一个产品仅属于一个工厂，工厂可以用单例模式。在具体工厂对应的工厂方法，创建具体产品。有利于新增产品，而不用新增工厂。
    - 我想，如果仅希望部分工厂新增生产产品品类，应该遵循接口分离原则，而不应让所有工厂新增工厂方法的实现。
- 原型模式（克隆模式）
    - [可以看这篇文章](https://www.cnblogs.com/lcngu/p/5388974.html)
    
### 结构型    
- 装饰者
    - [参考链接](https://www.jianshu.com/p/c26b9b4a9d9e)
- 适配器
    - 把不适用的原接口转换成能适用的目标接口，来达到类和类间协作的目的。 
    - [可以看这篇文章](https://www.jianshu.com/p/9d0575311214)
- 代理
    - [可以看这篇文章](https://www.jianshu.com/p/a8aa6851e09e)
### 行为型
- 观察者
- 访问者
- 迭代器


## TODO
- Android一些基类，少用继承，多用接口扩展；比如利用好ActivityLifecycleCallbacks