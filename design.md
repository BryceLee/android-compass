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
- 迪米特法则
    - 
- Thanks: [SOLID Principles : The Definitive Guide](https://android.jlelse.eu/solid-principles-the-definitive-guide-75e30a284dea)
## 设计模式
- 装饰者模式
    - [link](https://www.jianshu.com/p/c26b9b4a9d9e)