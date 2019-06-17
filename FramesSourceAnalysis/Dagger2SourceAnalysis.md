## Java注解基础：
- Todo
#### 准备
```
// Add Dagger dependencies
dependencies {
  compile 'com.google.dagger:dagger:2.x'
  annotationProcessor 'com.google.dagger:dagger-compiler:2.x'
}
```
#### 这里举一个电脑实体和零件实体的例子。
### @Inject&@Module
- 在Dagger2的使用中,我们可以选用@Inject或者@Module来构造对象，等于告诉Dagger2，
“来，等我需要的时候帮我new这个”，用@Inject来标记，需要被赋值的声明，“来，我这里需要对象，快帮我new”。另一个重要注解是@Component，它就像一条网线，把Dagger2 new的对象，“传送到”到需要被声明的地方。
- 如果用@Inject构造对象，我们来看下代码：
```
import javax.inject.Inject;//@Inject是java提供，并不是Dagger2

public class PartEntity {

  @Inject# 标记需要被构造的对象的构造方法（固定用法）
  public PartEntity() {
    System.out.println("PartEntity init");
  }

  public void name() {
    System.out.println("i am a part");
  }
}
```
```
import javax.inject.Inject;

public class ComputerEntity {

  @Inject
  PartEntity partEntity;# 标记需要被赋值的声明 （固定用法）

  public ComputerEntity() {
    # 连接网线，inject(this)完成注入，DaggerComputerComponent.builder().build()完成准备工作，后续源码分析将会提到。
    DaggerComputerComponent.builder().build().inject(this);
  }

  public PartEntity getPartEntity() {
    return partEntity;
  }

}
```
### 源码分析：
- 查看app/build/source/apt/debug/yourpackage(你的包名下)

- @Inject构造对象的局限性：1，无法构造参数(TODO)；2，无法构造那些第三方对象，因为我们无法给它的构造方法加注解。这时候需要用到@Module

Dagger2中如果用@Module提供两个同样类型的对象，Dagger2会报错：
```
@Module
public class CarModule {

  @Provides
  WheelEntity provideWheelA() {
    return new WheelModuleEntity("A");
  }

  @Provides
  WheelEntity provideWheelB() {
    return new WheelModuleEntity("B");
  }

}
error: com.android.dimodule.WheelEntity is bound multiple times:
  void inject(CarModuleEntity entity);
       ^
      @Provides com.android.dimodule.WheelModuleEntity com.android.dimodule.CarModule.provideWheelA()
      @Provides com.android.dimodule.WheelModuleEntity com.android.dimodule.CarModule.provideWheelB()
1 error

FAILURE: Build failed with an exception.
```


- 我们需要在指定@Provides的地方也指定一个@Named注解和注入@Inject的地方也指定一个@Named,目的在于告诉Dagger2把谁注入到谁的位置。看一下@Named的源代码：
```
import javax.inject.Named;
@Named
```
```
源码：
@Qualifier
@Documented
@Retention(RUNTIME)
public @interface Named {

    /** The name. */
    String value() default "";
}
``` 
- 使用例子：
```
@Module
public class CarModule {

  @Provides
  @Named("A")
  WheelEntity provideWheelA() {
    return new WheelEntity("A");
  }

  @Provides
  @Named("B")
  WheelEntity provideWheelB() {
    return new WheelEntity("B");
  }
}
- 在需要注入的位置也标记上@Named
public class CarEntity {

  @Inject
  @Named("A")
  WheelEntity wheelEntity;
  @Inject
  @Named("B")
  WheelEntity wheelEntity2;

  public CarEntity() {
    DaggerCarComponentModule.builder().carModule(new CarModule()).build().inject(this);
  }

  public WheelEntity getWheelEntity() {
    return wheelEntity;
  }

  public WheelModuleEntity getWheelEntity2() {
    return wheelEntity2;
  }

}

```
这样问题就解决来。

- 我们也可以自定义@Qualifier注解，
```
@Qualifier
@Retention(RetentionPolicy.RUNTIME)
public @interface QualifierA {

}
@Qualifier
@Retention(RetentionPolicy.RUNTIME)
public @interface QualifierB {

}
@Module
public class CarModule {

  @Provides
  @QualifierA
  WheelEntity provideWheelA() {
    return new WheelEntity("A");
  }

  @Provides
  @QualifierB
  WheelEntity provideWheelB() {
    return new WheelEntity("B");
  }

}
public class CarEntity {

  @Inject
  @QualifierA
  WheelEntity wheelEntity;
  @Inject
  @QualifierB
  WheelEntity wheelEntity2;

  public CarEntity() {
    DaggerCarComponentModule.builder().carModule(new CarModule()).build().inject(this);
  }

  public WheelEntity getWheelEntity() {
    return wheelEntity;
  }

  public WheelEntity getWheelEntity2() {
    return wheelEntity2;
  }

}
```
### 接下来我们来看看源代码，看Dagger2是如何实现帮我们注入对象的：
- 首先可以肯定的是，对象的创建和对象在哪里赋值都是我们指定的，我们通过@Injector来标记需要赋值的对象。用@Injector或者@Module来提供对象。通过定义接口XComponent来实现把提供的对象注入到需要的地方。在需要注入的地方，常见模块代码如下：
```
DaggerXComponent.Builder.xxModule(new XXModule()).build().inject(this);
```
#### Dagger2会根据我们自定义的接口XComponent生成对应的DaggerXComponent.java,
debug环境下生成的代码源于位于app/build/generated/source/apt/debug/yourpackage下，
#### 如果你使用@Module来提供对象：
```
public class CarEntity {

  @Inject
  WheelModuleEntity wheelEntity;
  @Inject
  WheelModuleEntity wheelEntity2;

  public CarEntity() {
    DaggerCarModuleComponent.builder().carModule(new CarModule()).build().inject(this);
  }

  public WheelModuleEntity getWheelEntity() {
    return wheelEntity;
  }

  public WheelModuleEntity getWheelEntity2() {
    return wheelEntity2;
  }
}
```
- 我们先来看，DaggerCarModuleComponent.builder().carModule(new CarModule()).build(),这段代码做了什么（后面我们再来说inject(this)做了什么）：

这里我们定义的接口是CarModuleComponent，
```
@Component(modules = CarModule.class)
public interface CarModuleComponent {

  void inject(CarEntity_Module entity);
}

```
生成的代码是：
```
public final class DaggerCarModuleComponent implements CarModuleComponent {
    public static final class Builder {
    private CarModule carModule;
    # 第一，创建Builder实例
    public static Builder builder() {
    return new Builder();
    }
    
    public static final class Builder {
    private CarModule carModule;
    private Builder() {}
    # 第三，我们构造DaggerCarModuleComponent(传入Builder实例)
    public CarModuleComponent build() {
      if (carModule == null) {
        this.carModule = new CarModule();
      }
      return new DaggerCarModuleComponent(this);
    }
    # 第二，我们通过类内部的Builder的carModule方法把被@Module注解修饰的类的实例传进来 
    public Builder carModule(CarModule carModule) {
      this.carModule = Preconditions.checkNotNull(carModule);
      return this;
    }
    }
  }
}

```

#### 我们再先看DaggerCardModuleComponent()的构造方法，


```
private DaggerCarComponent(Builder builder) {
    assert builder != null;
    initialize(builder);
  }

  @SuppressWarnings("unchecked")
  private void initialize(final Builder builder) {

    this.provideWheelAProvider =
        DoubleCheck.provider(CarModule_ProvideWheelAFactory.create(builder.carModule));

    this.carEntity_MembersInjector =
        CarEntity_MembersInjector.create(provideWheelAProvider);
  }
```

- 这时候就涉及到另一个生成类，就是被@Provides标记对象的生成类，这里的参数，就是我们手动传进来的，DaggerXCompent.Builder().xxmodule(new xxModule).build();
- 注意这个接口：
```
public interface Factory<T> extends Provider<T> {
}
```
```
public interface Provider<T> {
    T get();
}

```
```
public final class CarModule_ProvideWheelAFactory implements Factory<WheelEntity> {
  private final CarModule module;

  public CarModule_ProvideWheelAFactory(CarModule module) {
    assert module != null;
    this.module = module;
  }

  @Override
  public WheelModuleEntity get() {
    return Preconditions.checkNotNull(
        module.provideWheelA(), "Cannot return null from a non-@Nullable @Provides method");
  }

  public static Factory<WheelEntity> create(CarModule module) {
    return new CarModule_ProvideWheelAFactory(module);
  }
}

```
还涉及到需要被注入类的生成类：
```
public final class CarEntity_Module_MembersInjector implements MembersInjector<CarEntity_Module> {
  private final Provider<WheelModuleEntity> wheelEntityProvider;

  public CarEntity_Module_MembersInjector(Provider<WheelModuleEntity> wheelEntityProvider) {
    assert wheelEntityProvider != null;
    this.wheelEntityProvider = wheelEntityProvider;
  }

  public static MembersInjector<CarEntity_Module> create(
      Provider<WheelModuleEntity> wheelEntityProvider) {
    return new CarEntity_Module_MembersInjector(wheelEntityProvider);
  }

  @Override
  public void injectMembers(CarEntity_Module instance) {
    if (instance == null) {
      throw new NullPointerException("Cannot inject members into a null reference");
    }
    instance.wheelEntity = wheelEntityProvider.get();
  }

  public static void injectWheelEntity(
      CarEntity_Module instance, Provider<WheelModuleEntity> wheelEntityProvider) {
    instance.wheelEntity = wheelEntityProvider.get();
  }
}
```
- DaggerCarComponent的initialize()方法中调用了上面的XX_MembersInjector类的create()方法，里面有个重要方法：
```
 @Override
  public void injectMembers(CarEntity_Module instance) {
    if (instance == null) {
      throw new NullPointerException("Cannot inject members into a null reference");
    }
    instance.wheelEntity = wheelEntityProvider.get();
  }
```
- XX_MembersInjector返回了一个XX_MembersInjector的实例，给DaggerCardComponent做inject(需要被注入类的实例),
### 重点来来，DaggerCardComponent.Builder().xxmodule(new xxmodule).build()得到DaggerCardCompoent的实例，再调用inject(传入需要被注入的类的实例)，
```
@Override
  public void inject(CarEntity_Module entity) {
    carEntity_ModuleMembersInjector.injectMembers(entity);
  }
```
然后在这里，完成了注入：
```
 @Override
  public void injectMembers(CarEntity_Module instance) {
    if (instance == null) {
      throw new NullPointerException("Cannot inject members into a null reference");
    }
    instance.wheelEntity = wheelEntityProvider.get();
  }
```
### @Lazy
### @Binds
可以使用@Binds来简化@Provides 一个接口的对象
``` 
    @Provides
    IRepositoryManager bindRepositoryManager(RepositoryManager repositoryManager){
      return repositoryManager;
    };
```
--->
```
    @Binds
    abstract IRepositoryManager bindRepositoryManager(RepositoryManager repositoryManager);
    //前提是@Module修饰的类需要改为abstract
```
### Component.Builder
- 通过使用 Component.Builder 来注解接口，Dagger 会自动生成跟上面完全相同的 Builder 类。
### @BindsInstance
- 可以直接为与@Component相关联的@Module中的同类型的成员变量赋值，@Module中就不必要再提供已经被@BindsInstance修饰的变量。
### 说一千道一万，自己写一遍，才能明白
### @Module是如何工作的呢？

# Thanks
- https://zhuanlan.zhihu.com/p/24454466
- https://google.github.io/dagger/users-guide
- [Dagger 2 : Component.Builder注解有什么用？](https://juejin.im/post/5a4cf2b2f265da430d586ace)