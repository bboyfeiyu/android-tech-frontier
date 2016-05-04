## Android 双向Data Binding
>* 原文链接 : [2-way Data Binding on Android!](https://halfthought.wordpress.com/2016/03/23/2-way-data-binding-on-android/)
* 原文作者 : [halfthought](https://halfthought.wordpress.com/about/)
* 译文出自 : [开发技术前线 www.devtf.cn](http://www.devtf.cn/)
* 转载声明: 本译文已授权[开发者头条](http://toutiao.io/download)享有独家转载权，未经允许，不得转载!
* 译者 : [季高](https://github.com/canglangwenyue)
* 校对者: 
* 状态 : 校对中

随着Android Studio 2.1 Preview 3的发布，Android Data Binding现在已经支持双向双向绑定。

### Data Binding 快速回顾
作为对Data Binding的快速回顾，你现在可以添加通过表达式到你的layout文件，来引用你的data model中的变量。例如:

```xml
layout ...>
  <data>
    <variable type="com.example.myapp.User" name="user"/>
  </data>
  <RelativeLayout ...>
    <TextView android:text="@{user.firstName}" .../>
  </RelativeLayout>
</layout>
```

上面的代码bind user实例的first 那么 到TextView’s text field。无论何时user实例的data发生变化都会引起对应的UI 更新。

### 双向 Data Binding

Android 不能幸免于典型的数据实体操作，而且将变化从用户的输入反馈并写入model是很重要的。例如，上述的data是在一个关联表，如果把编辑过的text写回model，而无需从EditText上拉取数据将会很好。这里是你该如何做到这些：

```xml
<layout ...>
  <data>
    <variable type="com.example.myapp.User" name="user"/>
  </data>
  <RelativeLayout ...>
    <EditText android:text="@={user.firstName}" .../>
  </RelativeLayout>
</layout>
```

很灵活吧？这里唯一的不同是表达式被标记为 “@={}” 而不是 “@{}”.可以预见的是大多数的data binding依旧会是单向的，因为我们不想去创建所有的listeners去监听那些永远不会发生的变化。

### 隐式的listeners
你也可以引用attributes到其它View。

```java
<layout ...>
  <data>
    <import type="android.view.View"/>
  </data>
  <RelativeLayout ...>
    <CheckBox android:id="@+id/seeAds" .../>
    <ImageView android:visibility="@{seeAds.checked ? View.VISIBLE : View.GONE}" .../>
  </RelativeLayout>
</layout>
```

在上述代码中，无论何时CheckBox的选中状态发生变化，这个ImageView的可见性就会发生变化。而且这一操作并不需要你自己来attach 一个listener！这种类型的表达式仅仅是支持具有绑定表达式属性的attributes来实现双向data binding。

### 实现双向绑定
 
所以，你还需要什么呢？首先，你必须确定你的modelbuild.gradle中的gradle 插件的version是 2.1-alpha3或更新。

```java
classpath 'com.android.tools.build:gradle:2.1.0-alpha3'
```

And, of course, you need to enable data binding in your module’s android section of the build.gradle:
当然，你需要在你的android model 的build.gradle添加以下部分：

```java
android{
dataBinding.enabled = true}
```

### 收获？
所以，有什么收获呢？首先，最主要的是只有一些attributes被支持。这是因为没相应的listeners可以用来让data binding framework在一些事件发生变化后i拿到回到相应的回调处理。好消息是，已经支持的属性可能是你最关心的属性：

- AbsListView android:selectedItemPosition
- CalendarView android:date
- CompoundButton android:checked
- DatePicker android:year, android:month, android:day (yes, these are synthetic, but - we had a listener, so we thought you’d want to use them)
- NumberPicker android:value
- RadioGroup android:checkedButton
- RatingBar android:rating
- SeekBar android:progress
- TabHost android:currentTab (you probably don’t care, but we had the listener)
- TextView android:text
- TimePicker android:hour, android:minute (again, synthetic, but we had the listener)


如果你的变量名称和你的布局文件中的View id’s 冲突的话，从现在开始你将会得到警告。否则，我们如何得知的调用的执行者该是View或者是你的表达式的变量呢？

### 自己动手
假设上述列表的attribute不能满足你的需求。那么你该如何自己来实现呢？假设你有一个color picker View，你想让它实现双向data binding来进行颜色选择。

```java
public class ColorPicker extends View {
    public void setColor(int color) { /* ... */ }
    public int getColor() { /* ... */ }
    public void setOnColorChangeListener(OnColorChangeListener listener) { 
        /*...*/
    }
    public interface OnColorChangeListener {
        void onColorChange(ColorPicker view, int color);
    }
}
```

重要的部分是上述View有一个listener用以让data binding framework来实现监听。接下来，我们需要告诉data binding关于这个attribute。最简单的方式是用一个InverseBindingMethod在任何class上：

```java
@InverseBindingMethods({
  @InverseBindingMethod(type = ColorPicker.class, attribute = "color"),
})
```

这种情况下，getter方法的名称“getColor”和 “app:color.” 是相互匹配的。入股有一些不同的胡，你可以提供一个方法attribute来纠正它。这会创建一个动态的attribute，当绑定的时间使用后缀为“AttrChanged.”属性是会被调用。我们来看看如何设置listener:

```java
@BindingAdapter("colorAttrChanged")
public static void setColorListener(ColorPicker view,
        final InverseBindingListener colorChange) {
    if (colorChange == null) {
        view.setOnColorChangeListener(null);
    } else {
        view.setOnColorChangeListener(new OnColorChangeListener() {
            @Override
            public void onColorChange(ColorView view, int color) {
                colorChange.onChange();
            }
        });
    }
}
```

这是最简单的BindingAdapter，它是指定的color change listener 类型和 da的ta binding framework使用的listenerde一个转换作用，InverseBindingListener。然而，你经常也需要绑定监听colorChange事件，是吗？那么你将必须把listeners结合起来使用：

```java
@BindingAdapter(value = {"onColorChange", "colorAttrChanged"}, 
                requireAll = false)
public static void setColorListener(ColorPicker view,
        final OnColorChangeListener listener,
        final InverseBindingListener colorChange) {
    if (colorChange == null) {
        view.setOnColorChangeListener(listener);
    } else {
        view.setOnColorChangeListener(new OnColorChangeListener() {
            @Override
            public void onColorChange(ColorView view, int color) {
                if (listener != null) {
                    listener.onColorChange(view, color);
                }
                colorChange.onChange();
            }
        });
    }
}
```

现在你可以在ColorPicker View中使用双向data binding，并且绑定 onColorChange事件，甚至是在同一个View上。

#### INVERSEBINDINGADAPTERS
和BindingAdapters和setter方法​​类似，有些时候你需要做的不仅仅是调用一个getter方法来检索值。类如我们的ColorPicker如果有个包含一些颜色的枚举的话，我们需要将它转换成int：

```java
@InverseBindingAdapter(attribute = "color")
public static int getColorInt(ColorPicker view) {
    return ConvertColorEnumToInt(view.getColor());
}
```

#### PREVENTING LOOP
我们在使用双向data binding的时候遭遇的一个问题是一种无限循环。当改变一个value来发送一个事件时，绑定的listener将会把值设置给目标object。这将会引发另一个事件，并且可能会在一次触发View的变化和关闭我们的循环。这种循环通常是 View’s setter 或者 指定的 data value’s setter导致的，但是如果你没有控制 the View 或者 the data value的话，这将是不能保证的。我们决定通过BindingAdapters来阻止循环，在ColorPicker例子中：

```java
@BindingAdapter("color")
public static void setColor(ColorPicker view, int color) {
    if (color != view.getColor()) {
        view.setColor(color);
    }
}
```

这样就不会有更多的循环。

我希望你能了解到一些关于如何使用双向data binding的只是，并且如果你有任何issues，请让我知晓。

下一次我将会谈论事件的lambda表达式，以及我们的的发布会。


