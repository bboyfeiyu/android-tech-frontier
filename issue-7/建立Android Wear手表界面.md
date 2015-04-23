# Creating an Android Wear Watch Face

# 建立Android Wear手表界面


> * 原文链接 : [Creating an Android Wear Watch Face](http://code.tutsplus.com/tutorials/creating-an-android-wear-watch-face--cms-23718)
* 原文作者 : [ Paul Trebilcox-Ruiz](http://tutsplus.com/authors/paul-trebilcox-ruiz)
* [译文出自 :  开发技术前线 www.devtf.cn](www.devtf.cn)
* 译者 : [purecpatain](https://github.com/purecaptain) 
* 校对者: [这里校对者的github用户名](github链接)  
* 状态 :  未完成 / 校对中 / 完成 

One of the features that makes Android so special is the ability to customize every aspect of the user experience. When Android Wear first launched at Google I/O 2014, many developers and users found out that this wasn't exactly true for smart watches, as the official API for creating watch faces was noticeably missing. Given that the ability to make custom watch faces was one of the key wants from users, it's not surprising that developers discovered a way to create their own watch faces with an undocumented hack in Android Wear.

使开发Android如此独特的特点之一，是根据用户各个方面的体验来改造的能力。当Android Wear在2014年Google I/O大会上第一次发布的时候，许多开发者和用户认为这不是他们想要的智能手表，同时为开发手表界面的官方API明显没有。鉴于自定义手表界面的关键是用户的需要，这一点都不惊讶，开发者在没有文档的提前下破解了Android Wear，发现了创建属于自己手表界面的方法。

Luckily, Google quickly let everyone know that an official API was on its way and in December of 2014 that API was finally released to the development community. In this article, you're going to learn about the official Watch Faces API for Android Wear and implement a simple digital watch face that you will be able to expand on for your own needs. Implementing watch faces can be a bit verbose, but you can find the sample application for this article on [GitHub](https://github.com/tutsplus/AndroidWear-WatchFaces).

幸运地，Google不久后就让每个人知道官方API正在开发，在2014年12月，API最终在开发者社区发布了。在这篇文章，我们将学习关于Android Wear的官方手表界面API，实现一个你能够按你的需要扩展的电子手表界面。实现手表界面可能有一点冗杂，但是你能够在[GitHub](https://github.com/tutsplus/AndroidWear-WatchFaces)上找到这篇文章的简单应用。

## 1.Setting Up Your IDE

## 1.设置你的IDE

The first thing you're going to need to do to make your own watch face is get your project set up in Android Studio. When creating your project, select **Phone and Tablet** with a **Minimum SDK** of API 18 as Android 4.3 is the lowest version of the operating system to support bundled Android Wear applications. You will also need to check the **Wear** box with a **Minimum SDK** of API 21 selected. You can see an example of what your **Target Android Devices** screen should look like.

第一件你需要做的事，是在Android Studio中建立你自己的手表界面项目。
当你创建项目时，选择**Phone and Tablet**和**Minimum SDK**API18，作为操作系统Android 4.3的的最低版本，来支持捆绑Android Wear应用。你也将需要勾选Wear选项，并在**Minimum SDK**中选择API21.你可以看见一个例子，**Target Android Devices**窗口应该像这样。

![Target Android Devices](https://cms-assets.tutsplus.com/uploads/users/798/posts/23715/image/form-factors)

When you get to the two **Add an Activity** screens, select **Add No Activity** for both screens.

当你进入如下两个 **Add an Activity** 窗口，都选择 **Add No Activity**。 

![Add an activity to Mobile](https://cms-assets.tutsplus.com/uploads/users/798/posts/23715/image/nomobileactivity.jpg)

![Add an activity to Wear](https://cms-assets.tutsplus.com/uploads/users/798/posts/23715/image/nowearactivity.jpg)

Once you click **Finish**, your project environment should build and have a module for **mobile** and another one for **wear**.

一旦你点击了**Finish**，你的项目环境应该构建了，并且有一个**mobile**模块和另一个**wear**模块。

## 2.Building the Wear Watch Service

## 2.建立Wear Watch 服务

Android Wear implements watch faces through the use of `WatchFaceService`. In this article, you're going create an extension of the `CanvasWatchFaceService` class, which is an implementation of `WatchFaceService` that also provides a `Canvas` for drawing out your watch face. Start by creating a new Java class under the **wear** module in Android Studio that extends `CanvasWatchFaceService`.

Android Wear通过`WatchFaceService`的使用来实现手表界面。在这篇文章，你将建立一个`CanvasWatchFaceService`的扩展类，用来实现`WatchFaceService`，同样提供一个`Canvas`在手表界面上画画的类。在Android Studio中，开始在**wear**模块下建立一个新的Java类，继承`CanvasWatchFaceService`。

![Wear Watch Service](https://cms-assets.tutsplus.com/uploads/users/798/posts/23715/image/Screen-Shot-2015-03-27-at-2.09.53-AM.jpg)

~~~java
public class WatchFaceService extends CanvasWatchFaceService
~~~

Once you have your class, you're going to need to create an inner class, `WatchFaceEngine` in the source files of this article, that extends `Engine`. This is the watch face engine that handles system events, such as the screen turning off or going into ambient mode.

一旦你有了这个类，你将需要创建一个内部类，`WatchFaceEngine`在该文件下，继承`Engine`。这个是手表界面的处理系统事件的engin，例如窗口的跳转，或者进入ambient mode。

~~~java
private class WatchFaceEngine extends Engine
~~~

When your stub code for `WatchFaceEngine` is in, go back to the outer class and override the `onCreateEngine` method to return your new inner class. This will associate your watch face service with the code that will drive the display.

当你写好`WatchFaceEngine`的代码写入后，回到外部类，并覆写`onCreateEngine`方法，返回你新建的内部类。这将把你的手表界面服务用代码联系起来，将驱动显示。

~~~java
@Override
public Engine onCreateEngine() {
    return new WatchFaceEngine();
}
~~~

Once you have the bare bones service put together, you can move on to the general housekeeping tasks of updating your **AndroidManifest** files so that your service will be discoverable by Android Wear. Keep in mind that your current code won't do anything yet. We will come back to this class and flesh out the engine after doing some project housekeeping.

一旦你有了空的服务并放在了一起，你可以移到一般管理来尝试更新你的**AndroidManifest**文件，使你的服务能够被Android Wear发现。记住，你当前的代码是不工作的。我们将回到这个类，并做了一些项目管理后继续写engine。

## 3.Updating the AndroidManifest Files

## 3.更新Androidmanifest文件

Open the **AndroidManifest.xml** file in the **wear** module. Near the top you should already see a line that says:

在**wear**模块中打开**AndroidManifest.xml**文件。在靠近顶部处你应该已经看到这样一行：

~~~java
<uses-feature android:name="android.hardware.type.watch" />
~~~

Below that line, we need to add in the two required permissions for a watch face. These requirement are:

在这行下面，我们需要为手表界面增加两个需求权限。需求如下：

~~~java
<uses-permission android:name="com.google.android.permission.PROVIDE_BACKGROUND" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
~~~

Once your permissions are set, you will need to add a node for your service in the `application` node with permission to `BIND_WALLPAPER`, a few sets of `meta-data` containing reference images of your watch face for the selection screen (in this example we're just using the launcher icon), and an `intent-filter` to let the system know that your service is meant for displaying a watch face.

一旦你设置好了权限，在带`BIND_WALLPAPER`权限的`application`节点中，你需要为你的服务增加一个节点，为选择窗口，而一些包含图片的`meta-data`设置（在这个例子中，我们只用了安装图标），和一个`intent-filter`来让系统知道你的服务是用来显示手表界面的。

~~~java
<service android:name=".service.CustomWatchFaceService"
    android:label="Tuts+ Wear Watch Face"
    android:permission="android.permission.BIND_WALLPAPER">
 
    <meta-data
        android:name="android.service.wallpaper"
        android:resource="@xml/watch_face" />
    <meta-data
        android:name="com.google.android.wearable.watchface.preview"
        android:resource="@mipmap/ic_launcher" />
    <meta-data
        android:name="com.google.android.wearable.watchface.preview_circular"
        android:resource="@mipmap/ic_launcher" />
    <intent-filter>
        <action android:name="android.service.wallpaper.WallpaperService" />
        <category android:name="com.google.android.wearable.watchface.category.WATCH_FACE" />
    </intent-filter>
 
</service>
~~~

Once your **wear** manifest is complete, you will need to open the **AndroidManifest.xml** file in the **mobile** module and add in the two permissions we used in the **wear** module for `PROVIDE_BACKGROUND` and `WAKE_LOCK`, because Android Wear requires that both the **wear** and **mobile** modules request the same permissions for the wear APK to be installed on a user's watch. Once both manifest files are filled in, you can return to **CustomWatchFaceService.java** to start implementing the engine.

一旦你的**wear**清单完成了，你需要在**mobile**模块中打开**AndroidManifest.xml**文件，增加两个我们在**wear**模块`PROVIDE_BACKGROUND`和`WAKE_LOCK`中使用的权限，因为Android Wear要求**wear**和**mobile**模块请求相同的权限，用在用户的手表安装wear APK。当清单文件都填完整了，你可以回到**CustomWatchFaceService.java**来开始实现engine。

## 4.Start Your Engine

## 4.编写Engine

The `Engine` object associated with your service is what drives your watch face. It handles timers, displaying your user interface, moving in and out of ambient mode, and getting information about the physical watch display. In short, this is where the magic happens.

这个`Engine` 对象与你的驱动手表界面的服务相关联。它处理时钟，显示你的界面，进出ambient mode，并得到手表表面物理的信息。总而言之，这里是神奇之处。

###Step 1: Defining Necessary Values and Variables

###Step 1: 定义必要的值和变量

The first thing you're going to want to do is implement a set of member variables in your engine to keep track of device states, timer intervals, and attributes for your display.

第一件事就是在你的engine实现一些成员变量，来追踪设备状态，时间器间隔和显示的属性。

~~~java
//Member variables
private Typeface WATCH_TEXT_TYPEFACE = Typeface.create( Typeface.SERIF, Typeface.NORMAL );
 
private static final int MSG_UPDATE_TIME_ID = 42;
private long mUpdateRateMs = 1000;
 
private Time mDisplayTime;
 
private Paint mBackgroundColorPaint;
private Paint mTextColorPaint;
 
private boolean mHasTimeZoneReceiverBeenRegistered = false;
private boolean mIsInMuteMode;
private boolean mIsLowBitAmbient;
 
private float mXOffset;
private float mYOffset;
 
private int mBackgroundColor = Color.parseColor( "black" );
private int mTextColor = Color.parseColor( "red" );
~~~

As you can see, we define the `TypeFace` that we will use for our digital watch text as well as the watch face background color and text color. The `Time` object is used for, you guessed it, keeping track of the current device time. `mUpdateRateMs` is used to control a timer that we will need to implement to update our watch face every second (hence the 1000 milliseconds value for `mUpdateRateMs`), because the standard `WatchFaceService` only keeps track of time in one minute increments. `mXOffset` and `mYOffset` are defined once the engine knows the physical shape of the watch so that our watch face can be drawn without being too close to the top or left of the screen, or being cut off by a rounded corner. The three boolean values are used to keep track of different device and application states.

如你所见，我们定义了`TypeFace`，我们在数字手表文件上使用，同时也用在在手表界面的背景颜色和文本颜色上。
`Time`对象用来，你猜一下，用来追踪当前设备时间。`mUpdateRateMs`用来控制我们实现的每秒更新手表界面的时间器（`mUpdateRateMs`的值1000ms），因为标准`WatchFaceService`只追踪在每分钟内的增量。`mXffset`和`mYffset`被定义用来，在engine监测手表的物理图形的同时，使得我们的手表界面能够画出来，不太靠近屏幕的顶部或左边，或者被剪切成一个圆角。这里有是三个用来追踪不同设备和应用状况的boolean类型的值，


The next object you will need to define is a broadcast receiver that handles the situation where a user may be traveling and change time zones. This receiver simply clears out the saved time zone and resets the display time.

下一个你需要定义的对象是一个broadcast接收器，用来处理用户在旅行的情况，并改变时区。这个接收器简单的清除保存的时区，重置显示的时间。

~~~java
final BroadcastReceiver mTimeZoneBroadcastReceiver = new BroadcastReceiver() {
    @Override
    public void onReceive(Context context, Intent intent) {
        mDisplayTime.clear( intent.getStringExtra( "time-zone" ) );
        mDisplayTime.setToNow();
    }
};
~~~

After your receiver is defined, the final object you will need to create at the top of your engine is a `Handler` to takes care of updating your watch face every second. This is necessary because of the limitations of `WatchFaceService` discussed above. If your own watch face only needs to be updated every minute, then you can safely ignore this section.

在接收器定义后，最后一个创建的对象在engine的顶部，是一个`Handler`，用来处理每秒更新手表界面。这是很有必要的，因为以上讨论的`WatchFaceService`的限制。如果你自己的手表界面只需要每分钟更新一次，你就可以忽视这个部分。

~~~java
private final Handler mTimeHandler = new Handler() {
    @Override
    public void handleMessage(Message msg) {
        switch( msg.what ) {
            case MSG_UPDATE_TIME_ID: {
                invalidate();
                if( isVisible() && !isInAmbientMode() ) {
                    long currentTimeMillis = System.currentTimeMillis();
                    long delay = mUpdateRateMs - ( currentTimeMillis % mUpdateRateMs );
                    mTimeHandler.sendEmptyMessageDelayed( MSG_UPDATE_TIME_ID, delay );
                }
                break;
            }
        }
    }
};
~~~

The implementation of the Handler is pretty straightforward. It first checks the message ID. If matches `MSG_UPDATE_TIME_ID`, it continues to invalidate the current view for redrawing. After the view has been invalidated, the `Handler` checks to see if the screen is visible and not in ambient mode. If it is visible, it sends a repeat request a second later. The reason we're only repeating the action in the `Handler` when the watch face is visible and not in ambient mode is that it can be a little battery intensive to keep updating every second. If the user isn't looking at the screen, we simply fall back on the `WatchFaceService` implementation that updates every minute.

Handler的实现是简单漂亮的。它首先检查信息ID。如果匹配`MSG_UPDATE_TIME_ID`，它继续废除当前的视图，然后重画。在视图废除后，`Handler`检查屏幕是可见的或不在ambient mode。如果是可见的，它发送一个重复的请求下一秒。推断，我们只要在`Handler`中重复动作，当手表界面可见并不在ambient mode，它能够在只有一点点电量时正常刷新每一秒。如果用户没有看屏幕，我们后退到`WatchFaceServcice`的启用，每分钟更新。

###Step 2: Initializing the Engine
###Step 2: 初始化Engine

Now that your variables and objects are declared, it's time to start initializing the watch face. `Engine` has an `onCreate` method that should be used for creating objects and other tasks that can take a significant amount of time and battery. You will also want to set a few flags for the `WatchFaceStyle` here to control how the system interacts with the user when your watch face is active.

现在你的变量和对象都定义好了，现在开始初始化手表界面。`Engine`有一个`onCreate`方法，被用来创建对象和
其他能够获得一些重要的时间和电量的任务。你也将为`WatchFaceStyle`在这里设置一些标志，当手表界面活动时，来控制系统怎样和用户交流。

~~~java
@Override
public void onCreate(SurfaceHolder holder) {
    super.onCreate(holder);
 
    setWatchFaceStyle( new WatchFaceStyle.Builder( CustomWatchFaceService.this )
                    .setBackgroundVisibility( WatchFaceStyle.BACKGROUND_VISIBILITY_INTERRUPTIVE )
                    .setCardPeekMode( WatchFaceStyle.PEEK_MODE_VARIABLE )
                    .setShowSystemUiTime( false )
                    .build()
    );
 
    mDisplayTime = new Time();
~~~

For the sample app, you'll use `setWatchFaceStyle` to set the background of your notification cards to briefly show if the card type is set as interruptive. You'll also set the peek mode so that notification cards only take up as much room as necessary.

在示例的app中，如果卡片形式的设置是间断的，你将使用`setWatchFaceStyle`来设置你的通知标签的背景，并简洁的展示。你也将设置瞥一眼的模式，使通知卡片仅占据只需要的空间。

Finally, you'll want to tell the system to not show the default time since you will be displaying it yourself. While these are only a few of the options available, you can find even more information in the [official documentation](http://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html) for the `WatchFaceStyle.Builder` object.

最后，你将想要告诉系统不要显示默认的时间，因为你想按自己的方式显示。这些只是小部分的可选的选项，对于`WatchFaceStyle.Builder`对象，你可以在 [official documentation](http://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html)上找到更多的信息。

After your `WatchFaceStyle` has been set, you can initialize mDisplayTime as a new `Time` object.

在`WatchFaceStyle`设置后好，你可以初始化mDisplayTime作为一个新的`Time`对象。

`initBackground` and `initDisplayText` allocate the two `Paint` objects that you defined at the top of the engine. The background and text then have their color set and the text has its typeface and font size set, while also turning on anti-aliasing.

`initBackground`和`initDisplayText`分配两个你在engine顶部定义的`Paint`对象。背景和文本会有它们自己的颜色设置，文本有它自己的文字格式和字体大小设置，同样也开启抗锯齿。

~~~java
private void initBackground() {
    mBackgroundColorPaint = new Paint();
    mBackgroundColorPaint.setColor( mBackgroundColor );
}
 
private void initDisplayText() {
    mTextColorPaint = new Paint();
    mTextColorPaint.setColor( mTextColor );
    mTextColorPaint.setTypeface( WATCH_TEXT_TYPEFACE );
    mTextColorPaint.setAntiAlias( true );
    mTextColorPaint.setTextSize( getResource用户s().getDimension( R.dimen.text_size ) );
}
~~~

###Step 3: Handling Device State

Next, you need to implement various methods from the `Engine` class that are triggered by changes to the device state. We'll start by going over the `onVisibilityChanged` method, which is called when the user hides or shows the watch face.

接下来，你需要实现在`Engine`类中的多种方法，这些都通过对设备状态变化而触发。我们开始实现`onVisibilityChanged`方法，这是当用户隐藏或显示手表界面时调用的。

~~~java
@Override
public void onVisibilityChanged( boolean visible ) {
    super.onVisibilityChanged(visible);
 
    if( visible ) {
        if( !mHasTimeZoneReceiverBeenRegistered ) {
 
            IntentFilter filter = new IntentFilter( Intent.ACTION_TIMEZONE_CHANGED );
            CustomWatchFaceService.this.registerReceiver( mTimeZoneBroadcastReceiver, filter );
 
            mHasTimeZoneReceiverBeenRegistered = true;
        }
 
        mDisplayTime.clear( TimeZone.getDefault().getID() );
        mDisplayTime.setToNow();
    } else {
        if( mHasTimeZoneReceiverBeenRegistered ) {
            CustomWatchFaceService.this.unregisterReceiver( mTimeZoneBroadcastReceiver );
            mHasTimeZoneReceiverBeenRegistered = false;
        }
    }
 
    updateTimer();
}
~~~

When this method is called, it checks to see whether the watch face is visible or not. If the watch face is visible, it looks to see if the `BroadcastReceiver` that you defined at the top of the `Engine` is registered. If it isn't, the method creates an `IntentFilter` for the `ACTION_TIMEZONE_CHANGED` action and registers the `BroadcastReceiver` to listen for it.

调用此方法时，它检查是否手表界面是可见或不可见的。如果手表界面是可见的，你定义在`Engine`顶部的BroadcastReceiver会被注册。如果不是，该方法创建一个`ACTION_TIMEZONE_CHANGED`的动作`IntentFilter`，并注册`BroadcastReceiver`监听它。

~~~java
private void updateTimer() {
    mTimeHandler.removeMessages( MSG_UPDATE_TIME_ID );
    if( isVisible() && !isInAmbientMode() ) {
        mTimeHandler.sendEmptyMessage( MSG_UPDATE_TIME_ID );
    }
}
~~~

###Step 4: Cooperating With the Wearable Hardware

###Step 4: 与可穿戴设备的交互

When your service is associated with Android Wear, `onApplyWindowInsets` is called. This is used to determine if the device your watch face is running on is rounded or squared. This lets you change your watch face to match up with the hardware.

当你的设备与Android Wear相关联时，`onApplyWindowInsets`会被调用。这通常用来确定，你的手表界面设备上运行的是圆的还是方的。

When this method is called in the sample application, this method simply checks the device shape and changes the x offset used for drawing the watch face to make sure your watch face is visible on the device.

当这个方法在示例程序中被调用，这个方法是检查设备的形状和变化的x偏移量，用来画手表界面来确保在装置上你的界面是可见的。

~~~java
@Override
public void onApplyWindowInsets(WindowInsets insets) {
    super.onApplyWindowInsets(insets);
 
    mYOffset = getResources().getDimension( R.dimen.y_offset );
 
    if( insets.isRound() ) {
        mXOffset = getResources().getDimension( R.dimen.x_offset_round );
    } else {
        mXOffset = getResources().getDimension( R.dimen.x_offset_square );
    }
}
~~~

The next method that you will need to override is `onPropertiesChanged`. This method is called when the hardware properties for the Wear device are determined, for example, if the device supports burn-in protection or low bit ambient mode.

你还需要覆写的方法是`onPropertiesChanged`。当穿戴设备的硬件属性检测完成后，这个方法会被调用，例如，如果设备支持老化的保护或低速ambient mode。

In this method, you check if those attributes apply to the device running your watch face and save them in a member variable defined at the top of your `Engine`.

在这个方法中，你检查这些属性是否适用于设备运行手表界面，并保存在你的`Engine`类中被定义的成员变量。

~~~java
@Override
public void onPropertiesChanged( Bundle properties ) {
    super.onPropertiesChanged( properties );
 
    if( properties.getBoolean( PROPERTY_BURN_IN_PROTECTION, false ) ) {
        mIsLowBitAmbient = properties.getBoolean( PROPERTY_LOW_BIT_AMBIENT, false );
    }
}
~~~

###Step 5: Conserving Battery in Ambient and Muted Modes

###Step 5: 保护环境和静音模式中的电池

After you handle the initial device states, you will want to implement `onAmbientModeChanged` and `onInterruptionFilterChanged`. As the name implies, `onAmbientModeChanged` is called when the device moves in or out of ambient mode.

在你处理初始设备状态后，你将要实现`onAmbientModeChanged`和`onInterruptionFilterChanged`。正如名字显示的，当设备是否处于ambient mode，`onAmbientModeChanged`方法会被调用。

If the device is in ambient mode, you will want to change the color of your watch face to be black and white to be mindful of the user's battery. When the device is returning from ambient mode, you can reset your watch face's colors. You will also want to be mindful of anti-aliasing for devices that request low bit ambient support. After all of the flag variables are set, you can cause the watch face to invalidate and redraw, and then check if the one second timer should start.

如果设备处于ambient mode，你将要改变手表界面的颜色，使用黑白色来注意用户的电量。当设备从ambient mode中出回归，你可以重新设置手表界面的颜色。你也要注意对设备要求低的环境中支持抗锯齿。在所有的标志变量设置后，你能使手表界面失效和重绘，然后检查是否定时器应该开始。

~~~java
@Override
public void onAmbientModeChanged(boolean inAmbientMode) {
    super.onAmbientModeChanged(inAmbientMode);
 
    if( inAmbientMode ) {
        mTextColorPaint.setColor( Color.parseColor( "white" ) );
    } else {
        mTextColorPaint.setColor( Color.parseColor( "red" ) );
    }
 
    if( mIsLowBitAmbient ) {
        mTextColorPaint.setAntiAlias( !inAmbientMode );
    }
 
    invalidate();
    updateTimer();
}
~~~
![Wear Face](https://cms-assets.tutsplus.com/uploads/users/41/posts/23718/image/screen-1.jpg)

`onInterruptionFilterChanged` is called when the user manually changes the interruption settings on their wearable. When this happens, you will need to check if the device is muted and then alter the user interface accordingly. In this situation, you will change the transparency of your watch face, set your `Handler` to only update every minute if the device is muted, and then redraw your watch face.

当用户在可穿戴设备中手动改变中断的设置，`onInterruptionFilterChanged `会被调用。当调用时，你需要检查
如果该设备是无响应的，然后相应地改变用户界面。在这种情况下，你会改变手表界面的透明度，如果该设备是无响应的，设置你的`Handler`，只要每分钟更新，然后重画你的手表界面。

~~~java
@Override
public void onInterruptionFilterChanged(int interruptionFilter) {
    super.onInterruptionFilterChanged(interruptionFilter);
 
    boolean isDeviceMuted = ( interruptionFilter == android.support.wearable.watchface.WatchFaceService.INTERRUPTION_FILTER_NONE );
    if( isDeviceMuted ) {
        mUpdateRateMs = TimeUnit.MINUTES.toMillis( 1 );
    } else {
        mUpdateRateMs = DEFAULT_UPDATE_RATE_MS;
    }
 
    if( mIsInMuteMode != isDeviceMuted ) {
        mIsInMuteMode = isDeviceMuted;
        int alpha = ( isDeviceMuted ) ? 100 : 255;
        mTextColorPaint.setAlpha( alpha );
        invalidate();
        updateTimer();
    }
}
~~~

When your device is in ambient mode, the `Handler` timer will be disabled. Your watch face can still update with the current time every minute through using the built-in `onTimeTick` method to invalidate the `Canvas`.

当你的设备在ambient mode下，`Handler`定时器会无响应。通过使用定义的`onTimeTick`方法，你的手表界面仍然可以在根据当前时间每分钟更新。

~~~java
@Override
public void onTimeTick() {
    super.onTimeTick();
 
    invalidate();
}
~~~

###Step 6: Drawing the Watch Face

###Step 6: 描绘手表界面

Once all of your contingencies are covered, it's time to finally draw out your watch face. `CanvasWatchFaceService` uses a standard `Canvas` object, so you will need to add `onDraw` to your `Engine` and manually draw out your watch face.

一旦你所有的突发事件处理后，最后就可以画出的你的手表界面了。`CanvasWatchFaceService `用一个标准的`Canvas`对象，所以你将要为你的`Engine`增加`onDraw`，画出你的手表界面。

In this tutorial we're simply going to draw a text representation of the time, though you could change your `onDraw` to easily support an analog watch face. In this method, you will want to verify that you are displaying the correct time by updating your `Time` object and then you can start applying your watch face.

在本教程，我们只是要画一个时间的文本表示形式，虽然你可能会改变你的`OnDraw`来轻松支持模拟表盘。在这个方法中，你要确保你更新你的`Time`对象，然后你就可以开始使用你的手表界面来显示正确的时间。

~~~java
@Override
public void onDraw(Canvas canvas, Rect bounds) {
    super.onDraw(canvas, bounds);
 
    mDisplayTime.setToNow();
 
    drawBackground( canvas, bounds );
    drawTimeText( canvas );
}
~~~

`drawBackground` applies a solid color to the background of the Wear device.

`drawBackground`应用纯色背景来设置穿戴设备的背景。

~~~java
private void drawBackground( Canvas canvas, Rect bounds ) {
    canvas.drawRect( 0, 0, bounds.width(), bounds.height(), mBackgroundColorPaint );
}
~~~

`drawTimeText`, however, creates the time text that will be displayed with the help of a couple helper methods and then applies it to the canvas at the x and y offset points that you defined in `onApplyWindowInsets`.

drawtimetext是在两个辅助方法帮助下，创建时间的文本显示出来，并在你定义的`onapplywindowinsets`中，将其应用为X和Y的偏移点，。

~~~java
private void drawTimeText( Canvas canvas ) {
    String timeText = getHourString() + ":" + String.format( "%02d", mDisplayTime.minute );
    if( isInAmbientMode() || mIsInMuteMode ) {
        timeText += ( mDisplayTime.hour < 12 ) ? "AM" : "PM";
    } else {
        timeText += String.format( ":%02d", mDisplayTime.second);
    }
    canvas.drawText( timeText, mXOffset, mYOffset, mTextColorPaint );
}
 
private String getHourString() {
    if( mDisplayTime.hour % 12 == 0 )
        return "12";
    else if( mDisplayTime.hour <= 12 )
        return String.valueOf( mDisplayTime.hour );
    else
        return String.valueOf( mDisplayTime.hour - 12 );
}
~~~

![Wear Face](https://cms-assets.tutsplus.com/uploads/users/41/posts/23718/image/screen-2.jpg)

## Conclusion

## 结论

Once you've implemented the methods for drawing your watch face, you should be all set with the basic knowledge needed to go out and make your own watch faces. The nice thing about Android Wear watch faces is that this is just scratching the surface of what's possible.

一旦你实现了绘画手表界面的方法，你应该去学所需要的基本知识去做你自己的手表界面。关于Android Wear手表界面最棒的事是，实现任何可能实现的界面。

You can add companion configuration activities on the watch or on the phone, replace the `Canvas` based watch face with an OpenGL implementation, or derive your own class from WatchFaceService to meet your needs.

你可以添加活动在手表或手机上，取代基于OpenGL实现的`Canvas`，或根据watchfaceservice来写你自己的类，来满足你的需求。

Add to it that you can access other APIs or information from the user's phone and the possibilities seem endless. Get creative with your watch faces and enjoy.

去做吧，你可以从用户的手机中获得其他API或信息，可能似乎是永无止境的。享受于手表界面创建的过程。