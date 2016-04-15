Android N 带来的新通知栏
---

> * 原文链接 : [Android N: Introducing upgraded Notifications](https://medium.com/@hitherejoe/android-n-introducing-upgraded-notifications-d4dd98a7ca92#.u98ejhd43)
* 原文作者 : [Joe Birch](https://medium.com/@hitherejoe)
* 译文出自 : [开发技术前线 www.devtf.cn](http://www.devtf.cn)
* 转载声明: 本译文已授权[开发者头条](http://toutiao.io/download)享有独家转载权，未经允许，不得转载!
* 译者 : [rogero0o](https://github.com/)
* 校对者: [这里校对者的github用户名](github链接)  
* 状态 :  未完成 / 校对中 / 完成

# Android N: Introducing upgraded Notifications

# Android N 带来的新通知栏

![](https://cdn-images-1.medium.com/max/800/1*2dW-R23SdrBU507eUOTkAw.png)

After writing about the new [Picture-in-Picture](https://medium.com/@hitherejoe/android-n-introducing-picture-in-picture-for-android-tv-35f2392fb609#.2kdi3a170) feature for Android N, I decided to take a deep dive into another of the new features we saw released in the Developer Preview - notifications. Android N sees some great new additions to the notifications API, so lets take a look at some of these new features and how we can implement them into our Android applications.

在介绍过 Android N 新功能 [图中图](https://medium.com/@hitherejoe/android-n-introducing-picture-in-picture-for-android-tv-35f2392fb609#.2kdi3a170) 后，我决定深入研究另一个预览版中发布的新功能 - notifications。 Android N 增加了许多新的 notifications API ，让我们来看看增加了什么新功能，并且如何在我们的 app 中实现新功能。

![](https://cdn-images-1.medium.com/max/800/1*iiN8GLxeSQmlckQpk9O0PA.png)

To get a good grip of these new notification features I created Notifi, a simple Android project to demo all of the samples found in this article.

为了能方便的查看所有的新功能，我创建了一个 APP -Notifi ，一个简单的 Android project 用来展示所有的例子。

In Android N, notifications have been given a pretty big upgrade. We now have much more control over the look and feel of the notifications that can be used by our applications, and have also been given a bunch of new capabilities to enhance the experience for our user when dealing with these notifications. So what’s new? Well, this update introduces us to:

在 Android N 中，notifications 鸟枪换炮啦！ 现在在我们应用中对通知栏的展现，以及动画的控制都增强了许多，更被赋予了许多新的功能，在和 notifications 打交道时，用户体验将得到极大的提高 。 所以到底葫芦里卖的什么药呢？ 主要有以下几点：

* New system **layout templates** for notifications mean that by default they’re simpler and cleaner - making them feel much less cluttered.
* **Bundling** now means that notifications will no longer flood the users status bar - related notifications can now be shown under a single expandable notification group if desired.
* Using the new **inline reply action** our users can respond to textual actions from the notification itself, without the need to open our application and become distracted from what they’re currently engaged in.
* Custom views allow us to sensibly style our notifications to make them look exactly how we please.

* 新的系统 notifications 的 **Layout 模板** ，更加简单和简洁，使得 notifications 不再如此凌乱
* **Bundling**（绑定）意味着现在 notifications 不会再充斥着用户的状态栏了，相关的 notifications 现在可以在设计在一个可扩展的 notification group 中展示
* 使用新的 **内联回复动作** 使得用户可以直接在 notifications 中回复文本，不再需要打开别的 app 或者切换当前页面
* 新的自定义 views 使我们的 notifications 样式更加丰富多彩，更加让人喜欢

That all sounds great doesn’t it? So let’s get on with it and take a look at the implementation details for all of these.

这些看起来都很腻害对不对？所以让我们继续深入并且看看具体实现方法。

## Notification Templates

## Notification 模板

![](https://cdn-images-1.medium.com/max/800/1*2dW-R23SdrBU507eUOTkAw.png)

Android N sees the introduction of some new templates used when displaying notifications. These new templates rearrange the components which previously existed on previous SDK versions, making them look both simpler and cleaner. These new templates are automatically used by the system, so there’s no need to change your code for creating a notification.

Android N 介绍了一些在显示 notification 时使用的新的模板。这些新的模板重新排版了之前 SDK 版本的模板，使他们看起来更加的简洁清爽。这些新的模板将自动的被系统适配使用，所以我们不需要改变代码来创建一个 notification 。

Just like before we can implement a simple notification like so:

之前我们创建一个 notification 可能像是这样的：

	NotificationCompat.Builder builder = new NotificationCompat.Builder(context)
        .setSmallIcon(R.drawable.ic_phonelink_ring_primary_24dp)
        .setContentTitle(title)
        .setContentText(message)
        .setLargeIcon(largeIcon)
        .setAutoCancel(true);

Previously, displaying a notification using the code above (on a device pre-N) would look like this:

之前的版本，使用上述代码显示的 notification 效果将如下图：

![](https://cdn-images-1.medium.com/max/800/1*zt3znBCmLKHHQFPtbwng0Q.png)

Now on Android N, the new templates (with no changes to the code used above!) means our notifications now look like so:

现在在 Android N 上，新的模板（使用的代码并未改变）现在将展示成这样：

![](https://cdn-images-1.medium.com/max/800/1*oUPdd502MOUvYhW6evBO6A.png)

This new style of notification template not only looks cleaner, but I feel it helps to bring more focus onto the visual aspects and create less clutter in the small space that is available for notification content. Again, there’s nothing you need to change in your code - the system will automatically use this new look and feel by default.

这个新的 notification 样式模板看起来并不是那么简洁，但是我觉得这带来了更多的视觉方面的焦点并且减少了 notification 内容展示空间中令人分心的部分。并且，你完全不需要更改你的代码 - 系统将自动默认的显示新的样式。

Note: Remember you can and some style to your notification and set the colour used by your notification by simply using the setColor() method on your Builder instance:

注释：请记得你可以使用一些样式来设置你的 notification 并且通过在创建时调用 ``setColor()`` 来设置 notification 的颜色值。

	NotificationCompat.Builder builder = new NotificationCompat.Builder(context)
    // Other properties
    .setColor(ContextCompat.getColor(context, R.color.primary));

Using this will set the colour for the notification icon and title, as well as any actions you add to your notification.

使用上述代码将设置 notification 图标和标题的颜色，和添加到 notification 中的任何动作都是一样的。

![](https://cdn-images-1.medium.com/max/800/1*RrtA2TdJXR4RIK7hxgW27Q.png)

## Bundled notifications

## 绑定 notification

![](https://cdn-images-1.medium.com/max/800/1*D5ttwScwHbNbCvfuxdv08w.png)

With Android N, we now have the ability to bundle our notifications — this groups related notification items so that we can display multiple notifications under a single notification header. When you’re sending multiple related notifications in your application, you should be sure to group them so that you avoid flooding the users status bar with notifications. This can be pretty annoying for the user, so grouping any relevant notifications can help to improve the experience for your user.

在 Android N 中，我们可以绑定 notification 了，这是一组相关的 notification ，我们可以在一个单独的 notification 头部下显示多个 notification 。当你发送多个相关的 notification 时，你应该确认他们在一个组中来避免这些 notification 塞满了用户的状态栏。这会使用户感到非常的不友好，所以管理任何有关系的 notification 能明显的提升用户的体验。

So to implement bundled notifications we must begin by setting one of our notifications as the ‘group summary’ notification, this group summary notification will not appear in the stack of notifications but will be the only notification that is displayed on the device until expanded, acting as a header for our notification stack. Following notifications in the group will be what makes up the body of this notification stack.

所以为了实现绑定 notification 的功能我们首先需要设置一个我们的 notification 来当做 ‘小组概要’的 notification，这个小组概要的 notification 将不会出现在 notification 的堆栈中，但在点击前将是唯一显示在设备上的 notification ，点击展开后，将作为我们的 notification 栈头的唯一通知。组中的 notification 是这个 notification 栈的主要内容。

Note: You must set a notification as the group summary, otherwise your notifications will not appear as part of a group.

注意：你必须设置一个 notification 作为 ‘小组标题’，不然你的 notification 将不会以组的形式展现。

Next, we can group our notifications by simply assigning a group ID string using the setGroup() method when building a notification instance, like so:

接下来，我们可以在创建 notification 实例时使用 ``setGroup()`` 来设置一个 ``string`` 类型的组 ID 用来组织我们的 notification ，像这样：

	NotificationCompat.Builder builderOne = new NotificationCompat.Builder(context)
    // Other properties
    .setGroupSummary(true)
    .setGroup(KEY_NOTIFICATION_GROUP);

	NotificationCompat.Builder builderTwo = new NotificationCompat.Builder(context)
    // Other properties
    .setGroupSummary(true)
    .setGroup(KEY_NOTIFICATION_GROUP);

	NotificationCompat.Builder builderThree = new NotificationCompat.Builder(context)
    // Other properties
    .setGroupSummary(true)
    .setGroup(KEY_NOTIFICATION_GROUP);

This tells the notification manager that these notifications all belong to the same group, the system will then group these notifications into a bundle for us - it’s really as easy as that! This is a great way of reducing notifications shown in our users status bar, we can now group them under one notification to be viewed at the same time. Producing a result like below:

这将通知 notification manager 这些 notification 都是属于同一个组的，这样系统将会把这些 notification 为我们绑定到一起 - 就是这么简单！ 这是一个重新定义我们展示 notification 的绝妙的方法，现在我们能将他们组织到一个 notification 之下使他们能在同一时间被用户看到，产生的效果如下图所示：

![](https://cdn-images-1.medium.com/max/800/1*pz8l9ULVaDG1NLJJc5XmBA.gif)

## Actions for Bundled Notifications

## Bundled Notifications 的动作

![](https://cdn-images-1.medium.com/max/800/1*Gdl43jxYNcM3VNdY0LLHsg.png)

And if that wasn’t enough, just like standard notifications we can also still use actions for notifications that are part of a bundle. At first, these will be collapsed as part of the notification and not visible to the user - however, the user can reveal these actions by clicking the reveal arrow on the desired notification, as shown below:

如果这都不能满足你，就像标准的 notification 一样我们可以对 notification 使用动作，这也是绑定功能的一部分。首先，这些将会被折叠成 notification 的一部分并且不被用户所见 - 然而用户可以点击在描述 notification 上的箭头来揭示这些动作，就像下图所示：

![](https://cdn-images-1.medium.com/max/800/1*sqmkOms2FpwUhnz2ETusnA.gif)

These actions are handled exactly the same way as they are in the standard approach for notifications, so no extra work is needed. All we need to do is create our new Action instances and assign them to our notification like so:

这些动作将会以标准方法相同的处理方式作用在 notification 上，所以不需要任何额外的工作。所有我们需要做的都只是创建一个新的动作实例并且将它指向我们的 notification ， 像下面这样：

	PendingIntent archiveIntent = PendingIntent.getActivity(...);
	NotificationCompat.Action replyAction = ...;
	NotificationCompat.Action archiveAction = ...;

	NotificationCompat.Builder builder = new NotificationCompat.Builder(context)
	    // Other properties
	    .setGroup(KEY_NOTIFICATION_GROUP)
	    .addAction(replyAction)
	    .addAction(archiveAction);

## Direct Reply

## 直接回复

![](https://cdn-images-1.medium.com/max/800/1*UUL6jzM4N1Ehcn4mKe4UcA.png)

Using the RemoteInput API (which we’ve actually had since API level 4!), we can now show notifications in Android N that allow the user to respond directly from the notification without the need to open our app to do so. This is done by the display of an inline reply action, which essentially displays an additional button in our notification that allows the user to reveal a textual input field. This is great as it allows our user to interact almost frictionlessly with our application without having to open it, allowing them to remain undistracted from their current task and responding as they please.

使用 RemoteInput API (远程输入 API )(从 API level 4 开始就支持了) 我们就可以展示能让用户直接回复的 notification ，不需要让用户进入 app 来回复。这是通过显示一个内联回复动作来实现的，本质上来说就是显示一个添加按钮在 notification 上来允许用户输入直接输入回复文本。这使得用户与我们的 APP 相处的更加融洽，不需要经常打开应用，让用户更好的专注在正在处理的事情上。

When the user decides to interact with the inline reply action, the text that is submitted by the user is attached to our actions specified intent and is then sent to our app (be it activity, service etc), where we can retrieve said content and deal with it accordingly.

当用户使用 notification 内置回复动作时，文本将会被由我们动作指定的 intent 发送到我们的 app 中（可能是 activity,service 等等），在那我们获得用户输入的文本并进行下一步处理。

## Adding inline actions

## 添加 inline actions （内联回复行为）

So we’ve now learnt a little about this new inline reply action, but how can we implement it?! Well, in order to add a inline reply action to our notification, we need to begin by creating a new RemoteInput instance:

现在我们已经学习了关于新的内联回复行为，但是我们该如何实现呢？ 为了将内联回复加到 notificaiton ，第一步我们需要创建一个新的 RemoteInput 实例：

	RemoteInput remoteInput = new RemoteInput.Builder(KEY_TEXT_REPLY)
	    .setLabel(LABEL_REPLY)
	    .build();

The RemoteInput builder requires the use of an identification key (KEY_TEXT_REPLY in my case) that we can later use in our application to retrieve the text that has been entered in our inline reply action by the user. After that, we can then simply set the label to be used for the inline reply action which is displayed as the hint when the input field is in focus (and empty!). The submit icon for the inline reply action is automatically displayed within the notification for us by the system.

在创建 RemoteIput 实例时需要设置一个认证身份的 KEY （在上面代码中为 KEY_TEXT_REPLY）, 在后面我们需要在我们的应用中使用这个 KEY 来取回用户在 notification 中输入的文本。完成之后，我们就能简单的设置用于快速回复动作的提示语（也可以是空的）。快速回复的提交按钮已经被系统自动的显示在 notificaiton 中。

Next, we need to create the PendingIntent instances that we wish to launch when our actions are interacted with by the user. We do this just like we normally do when creating PendingIntents for use with our notification:

接下来，我们需要创建 PendingIntent 实例用来发送用户提交的数据。这些用于 notification 的 PendingIntent 与之前并没有太大的区别：

	PendingIntent replyIntent = PendingIntent.getActivity(context,
	        REPLY_INTENT_ID,
	        getDirectReplyIntent(context, LABEL_REPLY),
	        PendingIntent.FLAG_UPDATE_CURRENT);

	PendingIntent archiveIntent = PendingIntent.getActivity(context,
	        ARCHIVE_INTENT_ID,
	        getDirectReplyIntent(context, LABEL_ARCHIVE),
	        PendingIntent.FLAG_UPDATE_CURRENT);

	private static Intent getDirectReplyIntent(Context context, String label) {
	        return MessageActivity.getStartIntent(context)
	                .addFlags(Intent.FLAG_INCLUDE_STOPPED_PACKAGES)
	                .setAction(REPLY_ACTION)
	                .putExtra(CONVERSATION_LABEL, label);
	}

So now we have these PendingIntents, we actually need to make use of them! This is where our remoteInput instance also comes into play. We need to begin by creating our actions, so let’s take a look at our replyAction instance below:

现在我们创建了这些 PendingIntents ，自然要让他们用起来！这里是 remoteInput 实例使用的地方。我们需要开始创建回复动作，让我们来看看以下的 replayAction 实例：

	NotificationCompat.Action replyAction =
	        new NotificationCompat.Action.Builder(R.drawable.ic_reply,
	                LABEL_REPLY, replyIntent)
	                .addRemoteInput(remoteInput)
	                .build();

	NotificationCompat.Action archiveAction =
	        new NotificationCompat.Action.Builder(R.drawable.ic_archive,
	                LABEL_ARCHIVE, archiveIntent)
	                .build();

Here we create our action as normal and then use the addRemoteInput() method to attach our remoteInput instance to it. This is declaring that when this action is interacted with, we want to display the inline reply action to the user in our notification. The input data for this inline reply will be included in our replyIntent Intent once the inline reply has been submitted by the user.

这里我们创建一个普通的活动然后调用 addRemoteInput() 方法来把这个 remoteInput 实例添加到这个活动中。这里描述了当这个活动被触发时，我们希望给用户展示内联回复动作在 notification　中。当用户点击提交了快速回复的提交按钮时，输入的内容将会包含在我们的　replyIntent 中。

After that, we can simply add our actions to our Notification Builder like below (again, this is still the same as our previous approach Pre-N), no additional code is needed for the inline reply action implementation.

在这之后，我们可以简单的添加动作到 Notification Builder 像下面这样（这里再次像之前的版本一样），我们不需要为内联回复动作添加任何新的代码来实现他。

	NotificationCompat.Builder builder = new NotificationCompat.Builder(context)
	        .setSmallIcon(R.drawable.ic_phonelink_ring_primary_24dp)
	        .setContentTitle(title)
	        .setContentText(message)
	        .setLargeIcon(largeIcon)
	        .addAction(replyAction);
	        .addAction(archiveAction);
	        .setAutoCancel(true);

Once we’ve implemented the above, the outcome of the implementation will give us the result below:

一旦我们实现了以上的代码，那么实现的效果将会像如下展示：

![](https://cdn-images-1.medium.com/max/800/1*eN7DX3fRqgv1Pp-x0rN6mw.gif)

## Receiving data from inline actions

## 从内联动作中获取数据

So we’ve implemented the inline reply action and it looks great, but for it to be useful we need to actually retrieve the data submitted by the user. This is dead easy to do:

我们实现了内联回复动作并且这看起来很棒，但是为了让它变得有效我们需要获取到用户提交的数据。下面是个简单的例子：

	Bundle bundle = RemoteInput.getResultsFromIntent(intent);
	if (remoteInput != null) {
	    return bundle.getCharSequence(NotificationUtil.KEY_TEXT_REPLY);
	}

Here, we use the RemoteInput API to retrieve the input results from the intent. At this point, if results exist then we can retrieve our data from the bundle. We do so here by using the key we previously used to store the data, which is our KEY_TEXT_REPLY that we mentioned in the previous section.

在这里我们使用了 RemoteInput API 来重新得到 intent 中的输入数据。此时，如果返回结果存在，我们就能将数据从 bundle 中取得。我们通过使用之前用来存储数据的 key 来获取数据，在例子中就是之前提到的KEY_TEXT_REPLY 。

## Styling the inline reply

## 内联回复样式

We can also set the colour of our inline reply action by making use of the setColor() method when building our notification. Doesn’t it look great?

我们同样能设置内联回复动作的颜色，方法是在创建时使用 setColor() 方法来实现。这看起来是不是很不错？

![](https://cdn-images-1.medium.com/max/800/1*IYxS4XgRGaRXkr4DaShDeA.png)

You can also see this sets the colour for all of the components in our notification. Which can easily be achieved like so:

同时你也可以看到这设置了 notification 的所有组件。我们可以通过以下代码实现：

	NotificationCompat.Builder builder = new NotificationCompat.Builder(context)
	    // Other properties
	    .setColor(ContextCompat.getColor(context, R.color.primary));

## Heads-up notifications

## 浮动通知

![](https://cdn-images-1.medium.com/max/800/1*b9iSVM4oYFQjK-EvctVS7w.png)

Heads up notifications behave just as before in Marshmallow, however they now use the updated templates used by the system in Android N. This gives us a new notification look and feel as shown below:

浮动通知动作正如在 Marshmallow （棉花糖）版本中一样，然而现在他们在 Android N 中使用了升级后的模板。这赋予了我们一个新的 notification ，如下所示：

![](https://cdn-images-1.medium.com/max/800/1*VbJqGXop5ryAuccd98Or7g.gif)

The great thing about heads-up notifications in Android N is that we can now use Custom Layouts to declare how our notification should be displayed. We’ll cover that in the next section!

Android N 中的浮动通知很棒的功能是我们可以自定义 Layouts 来装饰我们的 notification ，下一章中我们将详细解析。

## Custom View notifications

## 自定义视图的 notification

As in previous versions of Android, we can still make use of custom layouts to use with our notifications. This means we can use our own layout resources to declare the look and feel of the notification (just don’t get carried away!). This is great for:

在 android 之前的版本中，我们仍然可以使用自定义布局来显示我们的 notification . 这意味着我们可以使用我们自己的布局资源来装饰 notification 的样子。这样的好处有：

* Displaying components that weren’t previously supported by the Notifications API
* Display more meaningful and glanceable information in your notification, fedding the user more meaningful knowledge without the need to open your app
* Adding branding to your notification to match the look and feel of your application


* 显示之前不被 Notifications API 支持的组件
* 在你的 notification 中显示更有意义、更方便查看的信息，给用户提供更有意义的信息而不需要打开应用
* 在你的 notification 中添加商标来让 notification 和你的应用统一外观

But with saying that, you should ensure to keep the design of your notifications to a minimum. We don’t want to overwhelm the user with information, especially in such a small space. Allow the user to benefit from your notification, whilst still keeping the detail and design to a minimum.

综上所述，你需要确认保持你的 notification 设计是最简洁的。我们不想要用过多的信息来淹没用户，特别是在这样小的一个空间。允许用户从你的 notification 中获取到他想要的，同时仍然保持细节和设计上的简洁。

So, with that in mind I think we’re ready to create a custom layout for our notifications! It’s fairly simple to do so, we can make use of the RemoteViews class to construct our custom notification.

所以，请牢记保持简洁，接下来我们就能创建一个自定义的 layout 了！其实这相当的简单，我们可以通过使用 RemoteViews 类来创建我们的自定义 notification .

	RemoteViews remoteViews = new RemoteViews(context.getPackageName(), R.layout.notif_custom_view);
	remoteViews.setImageViewResource(R.id.image_icon, iconResource);
	remoteViews.setTextViewText(R.id.text_title, title);
	remoteViews.setTextViewText(R.id.text_message, message);
	remoteViews.setImageViewResource(R.id.image_end, imageResource);

We begin by creating a new instance of this RemoteViews class, passing in the package name of our application (the system needs this when dealing with notifications) and the layout itself which we wish to use for the notification. We can then use the methods provided by the RemoteView class to set the data / resources used by the views in our layout. From the RemoteView class, I’ve made use of the following methods:

第一步，创建一个 RemoteView 的实例，传入我们的应用包名（系统处理 notifications 时需要用到），再传入我们需要应用到 notification 中的 layout 。接下来我们可以使用由 RemoteView 类提供的方法来设置 layout 中的 view 需要使用到的 数据/资源。在 RemoteView 类中，我使用了以下方法：

* setImageViewResource(viewId, resource) - The resource of our image and the view ID which we wish to use our image resource with.
* setTextViewText(viewId, text) - The text that we wish to display in the given text view, referenced by its ID.

* setImageViewResource(viewId, resource) - image 的 resource 和我们需要展示图片的 view 的 ID
* setTextViewText(viewId, text) - 我们需要展示的文本和需要展示文本的 view 的 ID

The class allows us to set data values, resources, listeners and more for our views. Be sure to check out the documentation for the complete list!

这个类允许我们设置给 view 的数据类型有 data values , resources , listeners 等等。如果想要查看完成的说明，请查阅文档。

So now we’ve created a RemoteViews instance, we need to use it with a notification to display our desired look and feel.

我们现在创建了一个 RemoteView 的实例，我们需要使用一个 notification 来展示我们想到达到的效果。

## Collapsed Views

## 折叠视图

In the previous section we declared the layout in the RemoteViews instance along with the data to use for those views, so when building our notification all we need to do is pass our RemoteViews instance to the setCustomContentView() method of our builder. In this exampe, we provide custom view for the collapsed (normal) state of our notification:

在上一章中我们将 RemoteView 实例中的 layout 和它的视图中使用的数据申明在一起，所以当我们创建 notification 时我们需要做的只是传递 RemoteView 实例到 builder 的 setCustomContentView() 这个方法中。在下面这个例子中，我们提供了折叠（正常）状态的 notification 的自定义视图：

	Notification.Builder builder = new Notification.Builder(context)
	        .setSmallIcon(R.drawable.ic_phonelink_ring_primary_24dp)
	        .setCustomContentView(remoteViews)
	        .setStyle(new Notification.DecoratedCustomViewStyle());
	        .setAutoCancel(true);

Setting a Custom Content View for the collapsed state of our notification will simply display our notification for this state alone. We haven’t set an expanded state, so the notification won’t be expandable and will be displayed as below:

为折叠状态的 notification 设置一个自定义视图将会单独展现一个折叠了的 noti1 . 我们还没设置展开的状态，所以 notification 将不会展开，效果如下图所示：

![](https://cdn-images-1.medium.com/max/800/1*q2zARCojn6qsG6ugYInbpg.png)

## Expanded Views

## 展开视图

Similar to above, we can also provide a custom view for the expanded (larger) state of our notification by passing a RemoteViews instance to the setCustomBigContentView() method:

如上所述，我们同样可以提供一个自定义视图给展开状态的 notification , 只需要传递一个 RemoteViews 实例到 setCustomBigContentView() 方法中：

	Notification.Builder builder = new Notification.Builder(context)
	        .setSmallIcon(R.drawable.ic_phonelink_ring_primary_24dp)
	        .setCustomBigContentView(bigRemoteView)
	        .setStyle(new Notification.DecoratedCustomViewStyle());
	        .setAutoCancel(true);

If we use the setCustomBigContentView() method without setting a collapsed layout using the setCustomContentView() method, then our notification will simply collapse to a state that only displays the application name, as below:

如果我们使用 setCustomBigContentView() 方法而没有用 setCustomContentView() 方法设置折叠状态的layout，这时我们的 notification 将会简单的折叠成一个只显示应用名称的状态，如下图所示：

![](https://cdn-images-1.medium.com/max/800/1*cNHPOkuFnaYJMoGuxHg6Fg.gif)

As a best practice, if you provide an expanded size notification layout then you should provide a collapsed layout. This is because the empty collapsed state above provides no context to the user, which goes against the point of a notification! If you’re only going to use a single size (collapsed or expanded) then you should just use the collapsed state by itself.

所谓最佳实践，如果你提供了展开大小的 notification 视图那么你应该也提供一个折叠状态的视图。这是因为没有折叠状态的视图将与用户没有任何的交互，这是与 notification 的设计初衷相违背的！如果你只用到展开或是折叠中的其中一个状态，那么你应该只使用折叠状态。

## Using both Collapsed and Expanded Views

## 使用能折叠和展开的视图

If we wish for our notification to be both expandable and collapsable, then we can provide RemoteView instances for both states by making calls to both setCustomContentView() and setCustomBigContentView() methods.

如果我们希望我们的 notification 又能折叠和展开，那么我们在创建 RemoteView 实例时就应该调用 setCustomContentView() 和 setCustomBigContentView() 这两个方法。

	Notification.Builder builder = new Notification.Builder(context)
	        .setSmallIcon(R.drawable.ic_phonelink_ring_primary_24dp)
	        .setCustomContentView(remoteView)
	        .setCustomBigContentView(bigRemoteView)
	        .setStyle(new Notification.DecoratedCustomViewStyle());
	        .setAutoCancel(true);

Now we’ve set both a collapsed and expanded view for our notification, the user can expand our notification to view more information about the notification.

现在我们设置了一个既能折叠又能展开的 notification ,用户能展开 notification 来查阅更加丰富的信息。

![](https://cdn-images-1.medium.com/max/800/1*Qnj3EfJHShve-vJjbkCd_g.gif)

## Custom Heads-up Views

## 自定义悬浮视图

We can also use custom layouts when it comes to heads-up notifications. Using the same approach in the previous section to create a heads-up notification, we can use a custom view by simply passing a RemoteViews instance in a call to the setCustomContentView() method.

我们同样能在悬浮 notification 中使用自定义视图。使用上一章中同样的方法来创建一个 notification ，我们可以简单的调用 setCustomContentView() 方法来设置 RemoteVews 的自定义视图。

![](https://cdn-images-1.medium.com/max/800/1*v3RPrWhAaGuO3yAfRtdy5w.gif)

## And that’s it!

## 最后

So we’ve taken a look at this new notifications in Android N and how to implement them — so now it’s time to build this into your existing / new applications ready for Android N! I think these are some great additions to the Android platform, not only are notifications more powerful (and prettier) but I’m looking forward to seeing better notification experiences within applications. If you have any questions, please feel free to tweet me or leave a response below!

我们简单的查阅了一个新的 notifications 在 Android N 中并且学习了怎么实现它，那么现在就把这些应用到你的旧的或是新的应用中去吧！我想这些新功能对于安卓平台来说也是棒棒的，这些 notifications 的新功能不仅强大，而且我还期待在应用中更加强大的 notification 展示。如果你有任何问题，请联系作者的 tweet 或是直接到[原文]((https://medium.com/@hitherejoe/android-n-introducing-upgraded-notifications-d4dd98a7ca92#.u98ejhd43)留言 ：）。

p.s don’t forget to hit the recommend button if you enjoyed this article :)

p.s 别忘了到[原文]((https://medium.com/@hitherejoe/android-n-introducing-upgraded-notifications-d4dd98a7ca92#.u98ejhd43)中点击推荐按钮哦！
