# Creating an Android Wear Watch Face

# 建立 Android Wear 手表界面


> * 原文链接 : [Creating an Android Wear Watch Face](http://code.tutsplus.com/tutorials/creating-an-android-wear-watch-face--cms-23718)
* 原文作者 : [ Paul Trebilcox-Ruiz](http://tutsplus.com/authors/paul-trebilcox-ruiz)
* [译文出自 :  开发技术前线 www.devtf.cn](www.devtf.cn)
* 译者 : [purecpatain](https://github.com/purecaptain) 
* 校对者: [这里校对者的github用户名](github链接)  
* 状态 :  未完成 / 校对中 / 完成 

One of the features that makes Android so special is the ability to customize every aspect of the user experience. When Android Wear first launched at Google I/O 2014, many developers and users found out that this wasn't exactly true for smart watches, as the official API for creating watch faces was noticeably missing. Given that the ability to make custom watch faces was one of the key wants from users, it's not surprising that developers discovered a way to create their own watch faces with an undocumented hack in Android Wear.

Android 有一个很强大的特性，就是可以自定义用户体验的每一个细节。在2014年 Google I/O 大会上，当 Android Wear 第一次发布的时候，许多开发者和用户认为这不是他们想要的智能手表，同时开发手表界面的官方API还没有发布。鉴于自定义的手表界面是用户的关键需求之一，开发者便在没有文档的帮助下找到了创建 Android Wear 个性化手表界面的方法。

Luckily, Google quickly let everyone know that an official API was on its way and in December of 2014 that API was finally released to the development community. In this article, you're going to learn about the official Watch Faces API for Android Wear and implement a simple digital watch face that you will be able to expand on for your own needs. Implementing watch faces can be a bit verbose, but you can find the sample application for this article on [GitHub](https://github.com/tutsplus/AndroidWear-WatchFaces).

还好，Google 很快发布声明说官方 API 处于开发之中，最终于2014年12月在开发者社区正式发布。在这篇文章中，我们将学习关于 Android Wear 的官方 API 来开发手表界面，实现一个能够自定义界面的电子手表。实现手表界面的方法可能有一点冗杂，你可以在[GitHub](https://github.com/tutsplus/AndroidWear-WatchFaces)上找到这篇文章示例应用。

## 1.Setting Up Your IDE

## 1.设置你的 IDE

The first thing you're going to need to do to make your own watch face is get your project set up in Android Studio. When creating your project, select **Phone and Tablet** with a **Minimum SDK** of API 18 as Android 4.3 is the lowest version of the operating system to support bundled Android Wear applications. You will also need to check the **Wear** box with a **Minimum SDK** of API 21 selected. You can see an example of what your **Target Android Devices** screen should look like.

首先，在 Android Studio 中建立 Android Wear 项目。在创建项目过程中，勾选 **Phone and Tablet**，并选择 **Minimum SDK** API18，作为操作系统 Android 4.3 的最低版本来支持捆绑的 Android Wear 应用。你也将需要勾选 Wear 选项，并在 **Minimum SDK** 中选择 API21.如下示例，**Target Android Devices** 窗口。

![Target Android Devices](https://cms-assets.tutsplus.com/uploads/users/798/posts/23715/image/form-factors)

When you get to the two **Add an Activity** screens, select **Add No Activity** for both screens.

在下两个 **Add an Activity** 窗口中，都选择 **Add No Activity**。 

![Add an activity to Mobile](https://cms-assets.tutsplus.com/uploads/users/798/posts/23715/image/nomobileactivity.jpg)

![Add an activity to Wear](https://cms-assets.tutsplus.com/uploads/users/798/posts/23715/image/nowearactivity.jpg)

Once you click **Finish**, your project environment should build and have a module for **mobile** and another one for **wear**.

点击 **Finish** ，项目环境就构建完成后会生成一个 **mobile** 模块和一个 **wear** 模块。

## 2.Building the Wear Watch Service

## 2.建立 Wear Watch Service

Android Wear implements watch faces through the use of `WatchFaceService`. In this article, you're going create an extension of the `CanvasWatchFaceService` class, which is an implementation of `WatchFaceService` that also provides a `Canvas` for drawing out your watch face. Start by creating a new Java class under the **wear** module in Android Studio that extends `CanvasWatchFaceService`.

Android Wear 通过使用 `WatchFaceService` 来实现手表界面。在这篇文章中，你将建立一个 `CanvasWatchFaceService` 的子类，它是一个提供了绘制手表界面 `Canvas ` 的 
 `WatchFaceService ` 实现。在 Android Studio 中，首先在 **wear** 模块下新建一个继承 `CanvasWatchFaceService` 的 Java 类。


![Wear Watch Service](https://cms-assets.tutsplus.com/uploads/users/798/posts/23715/image/Screen-Shot-2015-03-27-at-2.09.53-AM.jpg)

~~~java
public class WatchFaceService extends CanvasWatchFaceService
~~~

Once you have your class, you're going to need to create an inner class, `WatchFaceEngine` in the source files of this article, that extends `Engine`. This is the watch face engine that handles system events, such as the screen turning off or going into ambient mode.

有了这个类后，在该文件下创建一个继承 `Engine` 的内部类 `WatchFaceEngine`。在手表界面中，这个类是用来处理系统事件的工具，例如窗口的跳转，或者进入ambient mode。

~~~java
private class WatchFaceEngine extends Engine
~~~

When your stub code for `WatchFaceEngine` is in, go back to the outer class and override the `onCreateEngine` method to return your new inner class. This will associate your watch face service with the code that will drive the display.

完成 `WatchFaceEngine` 类后，回到外部类，覆写 `onCreateEngine` 方法，返回新建的内部类。用来关联手表界面服务和工具，并显示。

~~~java
@Override
public Engine onCreateEngine() {
    return new WatchFaceEngine();
}
~~~

Once you have the bare bones service put together, you can move on to the general housekeeping tasks of updating your **AndroidManifest** files so that your service will be discoverable by Android Wear. Keep in mind that your current code won't do anything yet. We will come back to this class and flesh out the engine after doing some project housekeeping.

一旦将空的服务和工具关联后，在一般管理中更新 **AndroidManifest** 文件，使服务能够被 Android Wear 发现。记住，当前程序还不能运行。回到上一个类，继续编写工具。

## 3.Updating the AndroidManifest Files

## 3.更新Androidmanifest文件

Open the **AndroidManifest.xml** file in the **wear** module. Near the top you should already see a line that says:

在 **wear** 模块中打开 **AndroidManifest.xml** 文件。在文件中应该看到这样一行：

~~~java
<uses-feature android:name="android.hardware.type.watch" />
~~~

Below that line, we need to add in the two required permissions for a watch face. These requirement are:

在这行下面，需要为手表界面增加两个请求权限。如下：

~~~java
<uses-permission android:name="com.google.android.permission.PROVIDE_BACKGROUND" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
~~~

Once your permissions are set, you will need to add a node for your service in the `application` node with permission to `BIND_WALLPAPER`, a few sets of `meta-data` containing reference images of your watch face for the selection screen (in this example we're just using the launcher icon), and an `intent-filter` to let the system know that your service is meant for displaying a watch face.

一旦设置好了权限，在有 `BIND_WALLPAPER` 权限的 `application` 节点中，为服务增加一个选择窗口节点，设置包含图片的节点 `meta-data` （在这个例子中，我们只用了安装图标），和一个 `intent-filter` 来让系统知道该服务是用来显示手表界面的。

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

当 **wear** 清单完成后，需要在 **mobile** 模块中打开 **AndroidManifest.xml** 文件，增加两个在 **wear** 模块中使用的权限 `PROVIDE_BACKGROUND` 和 `WAKE_LOCK`，因为当用户安装手表 APK 时，Android Wear 要求 **wear** 和 **mobile** 模块请求的权限时相同的。当文件都完成后，回到 **CustomWatchFaceService.java** 类来实现工具。

## 4.Start Your Engine

## 4.编写Engine

The `Engine` object associated with your service is what drives your watch face. It handles timers, displaying your user interface, moving in and out of ambient mode, and getting information about the physical watch display. In short, this is where the magic happens.

这个 `Engine` 对象是与驱动手表界面的服务相关联的。它处理定时器、显示界面、进出 ambient mode ，和得到手表的显示信息。总而言之，很特殊。

###Step 1: Defining Necessary Values and Variables

###Step 1: 定义必要的值和变量

The first thing you're going to want to do is implement a set of member variables in your engine to keep track of device states, timer intervals, and attributes for your display.

第一件事是在工具中实现一些成员变量，来追踪设备状态、时间间隔和显示的属性。

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

正如所见，定义 `TypeFace` ，用在数字手表文本中、手表界面的背景颜色和文本颜色上。 `Time` 对象是用来追踪当前设备的时间。因为标准的 `WatchFaceService` 只追踪每分钟的增量，所以定义 `mUpdateRateMs` 用来控制每秒更新手表界面的定时器（`mUpdateRateMs`的值1000ms）。定义 `mXffset` 和 `mYffset`是用来在工具监测手表物理图形的同时，使得手表界面的显示不太靠近屏幕的顶部或左边，或者被切成一个圆角。最后定义三个用来追踪设备和应用状况的 boolean 类型的值，


The next object you will need to define is a broadcast receiver that handles the situation where a user may be traveling and change time zones. This receiver simply clears out the saved time zone and resets the display time.

下一个定义的对象是一个 broadcast receiver，用来处理用户在旅行的情况下而改变时区。这个 receiver 清除保存的时区，重置显示的时间。

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

在 receiver 定义后，在工具中最后一个创建的对象是一个 `Handler` ，用来处理每秒更新手表界面。这是很有必要的，因为以上讨论的 `WatchFaceService` 是有限制的。如果定义的手表界面只需要每分钟更新一次，就可以忽视这个部分。

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

Handler 的实现很简单。它首先检查信息 ID。如果匹配 `MSG_UPDATE_TIME_ID`，就销毁当前的视图，然后重画。在视图销毁后，`Handler` 检查屏幕的可见性或是否在 ambient mode。如果是可见的，它发送一个请求，重复下一秒。由此，当手表界面可见并且不在ambient mode时，我们只要在 `Handler `中重复action 就可以了，只是要求用很少的电量来正常刷新每一秒。如果用户没有看屏幕，回到 `WatchFaceServcice` 的调用，进行每分钟的更新。

###Step 2: Initializing the Engine
###Step 2: 初始化Engine

Now that your variables and objects are declared, it's time to start initializing the watch face. `Engine` has an `onCreate` method that should be used for creating objects and other tasks that can take a significant amount of time and battery. You will also want to set a few flags for the `WatchFaceStyle` here to control how the system interacts with the user when your watch face is active.

现在变量和对象都定义好了，开始初始化手表界面。`Engine` 有一个 `onCreate` 方法，被用来创建对象和能够获得一些重要的时间和电量的任务。在这里，也将为 `WatchFaceStyle` 设置一些标志，当手表界面活动时，来控制系统和用户的交流。

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

在示例的 app 中，如果 card 形式的设置是间断的，就使用 `setWatchFaceStyle` 来设置通知标签的背景，并显示。也将设置 peek mode ，使通知 cards 占据尽少的空间。

Finally, you'll want to tell the system to not show the default time since you will be displaying it yourself. While these are only a few of the options available, you can find even more information in the [official documentation](http://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html) for the `WatchFaceStyle.Builder` object.

最后，当用户自定义方式显示时，就告诉系统不要显示默认的时间。这些只是一小部分可选的功能，对于 `WatchFaceStyle.Builder` 对象，你可以在 [official documentation](http://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html)上找到更多的信息。

After your `WatchFaceStyle` has been set, you can initialize mDisplayTime as a new `Time` object.

在 `WatchFaceStyle `设置后好，初始化 mDisplayTime 作为一个新的 `Time` 对象。

`initBackground` and `initDisplayText` allocate the two `Paint` objects that you defined at the top of the engine. The background and text then have their color set and the text has its typeface and font size set, while also turning on anti-aliasing.

`initBackground` 和 `initDisplayText` 会定义两个在工具中定义的`Paint`对象。背景和文本会有颜色设置，文本有文字格式和字体大小设置，同时也有抗锯齿设置。

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

接下来，需要实现在 `Engine` 类中的多个方法，它们都是根据设备状态的变化而触发。当用户隐藏或显示手表界面时调用并实现 `onVisibilityChanged` 方法。

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

调用此方法时，它检查手表界面是可见或不可见的。如果手表界面是可见的，你定义在 `Engine` 中的 BroadcastReceiver 会被注册。如果不是，该方法创建一个 `ACTION_TIMEZONE_CHANGED` 的动作`IntentFilter`，并注册 `BroadcastReceiver` 监听它。

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

当设备与 Android Wear 相关联时，`onApplyWindowInsets` 会被调用。这通常用来确定手表界面在设备上运行的图形。

When this method is called in the sample application, this method simply checks the device shape and changes the x offset used for drawing the watch face to make sure your watch face is visible on the device.

在示例程序中，调用这个方法是检查设备的形状和变化的x偏移量，用来画出手表界面，并确保界面在设备上是可见的。

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

还需要覆写的方法是 `onPropertiesChanged`。当穿戴设备的硬件属性检测完成后，这个方法会被调用，如果设备支持老化的保护或低速ambient mode，就启用。

In this method, you check if those attributes apply to the device running your watch face and save them in a member variable defined at the top of your `Engine`.

在这个方法中，检查这些属性是否适用于设备来运行手表界面，并保存在 `Engine` 类中被定义的成员变量。

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

在设备初始状态处理后，要实现 `onAmbientModeChanged` 和 `onInterruptionFilterChanged` 类。正如名字显示的，当设备是否处于 ambient mode 时，`onAmbientModeChanged` 方法会被调用。

If the device is in ambient mo工具de, you will want to change the color of your watch face to be black and white to be mindful of the user's battery. When the device is returning from ambient mode, you can reset your watch face's colors. You will also want to be mindful of anti-aliasing for devices that request low bit ambient support. After all of the flag variables are set, you can cause the watch face to invalidate and redraw, and then check if the one second timer should start.

如果设备处于 ambient mode，手表界面的颜色会被改变为黑白色来降低用户电量的流失。当设备从 ambient mode 中回归，重新设置手表界面的颜色。也要注意在设备要求低的环境中支持抗锯齿。在设置所有的标志变量后，调用重绘手表界面，最后检查定时器是否应该开启。

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

当用户在可穿戴设备中改变中断设置后，`onInterruptionFilterChanged` 会被调用。调用时，需要检查该设备是否响应的，而相应地改变用户界面。在这种情况下，改变手表界面的透明度，如果该设备是无响应的，设置 `Handler` 只要每分钟更新，然后重画你的手表界面。

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

当设备在 ambient mode 下，`Handler` 定时器会无响应。通过使用定义的 `onTimeTick` 方法，手表界面仍然可以根据当前的时间每分钟更新。

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

在所有的事件处理后，最后就可以绘制手表界面了。为了 `CanvasWatchFaceService` 能使用一个标准 `Canvas` 对象，需要为 `Engine` 增加 `onDraw`，并绘制手表界面。

In this tutorial we're simply going to draw a text representation of the time, though you could change your `onDraw` to easily support an analog watch face. In this method, you will want to verify that you are displaying the correct time by updating your `Time` object and then you can start applying your watch face.

在本教程中，只是画一个时间的文本表示形式，只要通过改变 `OnDraw` 来模拟动态的表盘变化。在这个方法中，要确保更新 `Time` 对象，然后就可以开始使用手表界面来显示正确时间了。

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

`drawBackground` 应用纯色来设置穿戴设备的背景。

~~~java
private void drawBackground( Canvas canvas, Rect bounds ) {
    canvas.drawRect( 0, 0, bounds.width(), bounds.height(), mBackgroundColorPaint );
}
~~~

`drawTimeText`, however, creates the time text that will be displayed with the help of a couple helper methods and then applies it to the canvas at the x and y offset points that you defined in `onApplyWindowInsets`.

`drawtimetext` 是在两个辅助方法帮助下，创建时间文本并显示的，然后在定义 `onapplywindowinsets` 中，将其应用为X和Y的偏移点，。

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

一旦实现了绘画手表界面的方法，就应该去学习做手表界面要学的基本知识。关于 Android Wear 手表界面最棒的事是就是能够自定义界面。

You can add companion configuration activities on the watch or on the phone, replace the `Canvas` based watch face with an OpenGL implementation, or derive your own class from WatchFaceService to meet your needs.

你可以在手表或手机上添加活动，取代基于OpenGL实现的`Canvas`，或根据watchfaceservice来写自己的类。

Add to it that you can access other APIs or information from the user's phone and the possibilities seem endless. Get creative with your watch faces and enjoy.

去做吧，从用户的手机中获得 APIs 和信息的潜力是无穷的。在手表界面的创建的过程中获得快乐吧。