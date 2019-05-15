![](https://user-gold-cdn.xitu.io/2019/3/11/1696b8d0fa3ca872?w=624&h=193&f=png&s=16235)
### 简单上手体验：
```
//app module下build.gradle:
android {
...
    dataBinding {
       enabled true
    }
}

//项目根目录下：
//gradle.properties:
android.databinding.enableV2=true
//build..gradle:
allprojects {
    repositories {
        google()
        jcenter()
    }
}
```
layout中的```<data>...</data>```就是数据模型，我们通过他来关联layout对应等数据，参数设置就是在data内设置```<variable></variable>```,例子如下：
```
//layout: 注意最外层不要设计宽高属性，会无法通过编译，提示duplicate attribute
<?xml version="1.0" encoding="utf-8"?>
<layout
  xmlns:android="http://schemas.android.com/apk/res/android"
  xmlns:app="http://schemas.android.com/apk/res-auto"
  >
  <data>
    <variable
      name="User"
      type="com.android.jetpacklearning.databing.User"></variable>
  </data>
  
  <android.support.constraint.ConstraintLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
      android:id="@+id/top"
      android:text="@{User.firstname}"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      app:layout_constraintBottom_toBottomOf="parent"
      app:layout_constraintLeft_toLeftOf="parent"
      app:layout_constraintRight_toRightOf="parent"
      app:layout_constraintTop_toTopOf="parent"/>
    <TextView

      android:text="@{User.lastname}"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      app:layout_constraintBottom_toBottomOf="parent"
      app:layout_constraintLeft_toLeftOf="parent"
      app:layout_constraintRight_toRightOf="parent"
      app:layout_constraintTop_toBottomOf="@id/top"/>
  </android.support.constraint.ConstraintLayout>
</layout>
```
```
//Activity相关代码
@Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_ac__data_bing);
    ActivityAcDataBingBinding dataBinding = DataBindingUtil
        .setContentView(this, R.layout.activity_ac__data_bing);
    User user = new User("小A", "小B");
    dataBinding.setUser(user);
    //只要给View设置Id,databing就帮我们找到了View，实例和Id近似
    dataBinding.topText.setOnClickListener(new OnClickListener() {
      @Override
      public void onClick(View v) {
        Toast.makeText(Ac_DataBing_Easy.this, "topText", Toast.LENGTH_SHORT).show();
      }
    })
  }
```

结果：

![](https://user-gold-cdn.xitu.io/2019/3/11/1696c11c3ee8cc7d?w=325&h=521&f=png&s=11237)

### 使用可观察数据实体

### Binding Adapter
自定义属性，
### Thanks
- [https://developer.android.com/topic/libraries/data-binding](https://developer.android.com/topic/libraries/data-binding)
- [CodeLabs](https://codelabs.developers.google.com/codelabs/android-databinding/#0)
- [Android Data Binding Library samples](https://github.com/googlesamples/android-databinding)
