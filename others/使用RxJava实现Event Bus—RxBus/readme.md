Implementing an Event Bus With RxJava - RxBus
---

>* 原文链接：[Implementing an Event Bus With RxJava - RxBus](http://nerds.weddingpartyapp.com/tech/2014/12/24/implementing-an-event-bus-with-rxjava-rxbus/)
> * 作者：[Kaushik Gopal](http://nerds.weddingpartyapp.com/authors/kaushik-gopal/) 
> * 译者：[FILWAndroid](https://github.com/FILWAndroid)
> * 校对者：

本博文有三部分：

 1. 快速入门什么是Event Bus
 2. 使用RxJava实现Event Bus
 3. 针对这种方式的一些想法

"RxBus"并不是打算作为一个库。使用RxJava实现一个Event Bus简单至极，它并不保证一个独立库的臃肿。

##Part1：什么是Event Bus

我们先谈谈两个相似的概念：Observer模式和Pub-sub模式

###Observer模式
这是一个开发模式，你的类或主要对象(称作Observable)通知相关信息(event)给其他感兴趣的类或对象(称作Observers)。

###Pub-sub模式
Pub-sub模式的目标与Observer模式完全相同。你想让一些类知道某些事情的发生。

不过Observer模式和Pub-sub模式之间有一个重要的语义区别：在Pub-sub模式中的关注点是向外”广播"消息。Observable并不关心知道事件到了谁那里，只要出去即可。换句话说，Observable(又称Publisher)不关心谁是Observers(又称Subscribers)。

###为什么匿名？
这样允许”解耦”，这是计算机编程中的一个优点。你应该在你的设计中尽可能地降低耦合。

典型地，你期望publisher直接知道它需要通知的每一个subscriber，从而一旦”event"或者消息准备好，它可以去通知每一个subscriber。但是在Event Bus中，publisher去除了这种职责以及依赖，因为publisher和subscriber不需要有逻辑代码去建立两者之间的依赖。

换句话说，任何时候都”有意识地解耦”你的代码。

###如何匿名？
好的，那么一个很自然的关于Pub-sub的问题就是：你如何去实现publisher和subcriber之间的匿名。一个简单的方式就是持有一个中间人，让中间人去关心所有的通信。Event Bus就是这样的一个中间人。

就这样，Event Bus就是这么简单。

在Android中有两个普遍应用的Event Bus库：[Otto](http://square.github.io/otto/)和Green Robot的[EventBus](https://github.com/greenrobot/EventBus)。有很多在线博文介绍如何在你的应用中使用它们。

##Part2：使用RxJava实现Event Bus

我已经在GitHub仓库上发布了在Android使用RxJava的[使用实例](https://github.com/kaushikgopal/Android-RxJava)，我会继续在这个仓库上发布完整的实现。这里是部分主要实现：

``` java
// 这是一个中间人对象
public class RxBus {

  private final Subject<Object, Object> _bus = new SerializedSubject<>(PublishSubject.create());

  public void send(Object o) {
    _bus.onNext(o);
  }

  public Observable<Object> toObserverable() {
    return _bus;
  }
}
```

就这样，你已经有了一个准备好的Event Bus去使用了。

这里是你如何向bus发布一个event：

```java
@OnClick(R.id.btn_demo_rxbus_tap)
public void onTapButtonClicked() {

    _rxBus.send(new TapEvent());
}
```

这里是你如何监听来自其他fragments/services等的events：

```java
// 注意：订阅到完全相同的用来发布事件的_rxBus实例是很重要的。
_rxBus.toObserverable()
    .subscribe(new Action1<Object>() {
      @Override
      public void call(Object event) {

        if(event instanceof TapEvent) {
          _showTapText();

        }else if(event instanceof SomeOtherEvent) {
          _doSomethingElse();
        }
      }
    });
```

在这个例子中，我们从顶部的fragment(绿色部分)发布events，然后在底部(蓝色部分)监听(通过bus)。
![DemoView](http://nerds.weddingpartyapp.com/images/posts/rxbus_simple.gif)

##part3 部分思考
###Dead events
在某些情况下，知道当前是否有observers监听bus是很有用的。例如，如果你正在[使用Event Bus去处理GCM推送通知](http://markhudnall.com/2013/11/13/gcm-foreground-and-background/)，并且不想给那些应用在前台的发送推送通知，那么监听”[dead events](https://github.com/square/otto/blob/master/otto/src/main/java/com/squareup/otto/DeadEvent.java)”就很重要了。

例如在Wedding Party的最近一个版本，我们添加了”Messaging”到我们的应用。如果用户已经打开了应用(因此至少有1个或多个bus的监听器)，我们不会发送推送通知。但是如果应用在后台，那么我们就会发送一个推送通知去让用户知道聊天消息。在Event Bus发布一个event之后，如果没有subscriber监听，一个dead event就会返回。如果我们得到了一个dead event，就会发出一个推送通知。

我们如何使用RxBus实现这一点？

这真的相当简单。Subject有一个非常有用的方法hasObservers()可以明确地告诉我们。这个方法是在[RxJava的1.x版本](https://github.com/ReactiveX/RxJava/pull/1802)添加的，因此你必须使用RxAndroid的最近版本(在写本博文时是0.23.0)才能够看到这个方法。

###我应该使用RxBus替代Otto/EventBus么？
如果你只是想在你的应用中使用Event Bus，你最好使用像[Otto](http://square.github.io/otto/)(我强烈推荐)或[EventBus](https://github.com/greenrobot/EventBus)这样的库。Otto的api简洁、依靠注解驱动，可能更容易使用。

如果你对Rx非常熟悉，已经在你的应用中使用RxJava了，并且不想添加一个额外的库，那肯定来试试用RxBus实现吧。

我有没有遗漏点什么？你有更多的关于RxBus实现的建议么？在[Reddit](http://www.reddit.com/r/androiddev/comments/2qebdo/rxbus_implementing_an_event_bus_with_rxjava/)、[Google Plus](https://plus.google.com/105979641354189463768/posts/Au1xZ8RGLdG)关注讨论，@[kaushikgopal](http://twitter.com/kaushikgopal)自由评论或者在[repo](https://github.com/kaushikgopal/Android-RxJava/issues)提出问题。

嘿，我还会有两篇博文来更深入的研究RxJava，我会让RxBus例子更好。请继续关注。