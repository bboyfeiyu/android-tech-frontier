`开始学习Material Design`
---

> * 原文链接 : [Android Getting Started with Material Design](http://www.androidhive.info/2015/04/android-getting-started-with-material-design/)
* 作者 : [Ravi Tamada](http://www.androidhive.info/)
* 译者 : [xu6148152](https://github.com/xu6148152) 
* 校对者: [这里校对者的github用户名](github链接)  
* 状态 :  已完成




You might have heard of android Material Design which was introduced in Android Lollipop version. In Material Design lot of new things were introduced like Material Theme, new widgets, custom shadows, vector drawables and custom animations. If you haven’t working on Material Design yet, this article will give you a good start.`

你可能已经听说过Android Lollipop中引入的Material Design.Material Design引入了很多新的东西，如Material Theme, new widgets, custom shadows, vector drawables and custom animations(要翻译？).如果你还没用过Material Design，那么本文很适合你。

In this tutorial we are going to learn the basic steps of Material Design development i.e writing the custom theme and implementing the navigation drawer using the RecyclerView.

Go through the below links which give you much knowledge over Material Design.

> Material Design Specifications

> Creating Apps with Material Design

我们将在本文中学习Material Design开发的基本步骤:编写自定义主题和用RecyclerView实现navigation drawer.

在下面的链接中，你能够学到更多有关Material Design的知识  
[Material Design Specifications](http://www.google.com/design/spec/material-design/introduction.html#)  
[Creating Apps with Material Design](http://developer.android.com/training/material/index.html)  
 
<a href="http://download.androidhive.info/download?code=WPSkdrdZprHT0KLCZS3ClafgXBikGqM4r7FnNYdsdUTmlAkK6%2F2mkT0heOlNOq4U82rzqbod%2F14yU2uk5TWY4Zp%2FAYx6oiD7SKI%2FEgtUapzQUqkqcWEXX1bmw%3D%3DvqARiMEKqkqsXGbVf3vVUoffTqQcD2qfqZo" target="_blank">DOWNLOAD CODE</a>(那个黑块怎么搞？)

###VIDEO DEMO  
<a href="http://www.youtube.com/embed/jDXX_wDvarM">video</a>(怎么引用youtube?markdown似乎不行啊) 

###Material Design Color Customization
Material Design Color Customization

Material Design provides set of properties to customize the Material Design Color theme. But we use five primary attributes to customize overall theme.

Material Deisgin提供以下系列自定义Material Deisign 颜色主题的属性，但本文只使用5种基本属性来自定义整体的主题

colorPrimaryDark – This is darkest primary color of the app mainly applies to notification bar background.
colorPrimaryDark - 这是app中最黑的基本色，主要用来做notification bar的背景.

colorPrimary – This is the primary color of the app. This color will be applied as toolbar background.
colorPrimary - 这是app的基本颜色,将用作toolbar的背景色

textColorPrimary – This is the primary color of text. This applies to toolbar title.
textColorPrimary - 文本颜色,用于toolbar的标题

windowBackground – This is the default background color of the app.
windowBackground - app默认的背景颜色

navigationBarColor – This color defines the background color of footer navigation bar.
navigationBarColor - 这个颜色定义了navigation bar页脚的背景色.  
<img src="http://cdn2.androidhive.info/wp-content/uploads/2015/04/android-material-design-color-schema.png?524b4b" alt="android-material-design-color-schema" width="720px" height="auto" class="alignnone size-full wp-image-38236">

You can go through this material design color patterns and choose the one that suits your app.
你能选择适合你APP风格的颜色

Downloading Android Studio
###下载as
Before going further, download the Android Studio and do the necessary setup as I am going to use Android Studio for all my tutorial from now on. If you are trying the Android Studio for the first time, go the overview doc to get complete overview of android studio.
再往下走之前，先现在AS，并配置好本文之前提到的东西，如果你是首次使用AS，先去看看文档.

Creating Material Design Theme
###创建Material Design 主题
1. In Android Studio, go to File ⇒ New Project and fill all the details required to create a new project. When it prompts to select a default activity, select Blank Activity and proceed.
1. 在as中，创建新项目,选择BlankActivity.

2. Open res ⇒ values ⇒ strings.xml and add below string values.
1. 打开res->values->strings.xml 添加以下字符值

strings.xml  
<resources>  

    <string name="app_name">Material Design</string>
    <string name="action_settings">Settings</string>
    <string name="action_search">Search</string>
    <string name="drawer_open">Open</string>
    <string name="drawer_close">Close</string>
 
    <string name="nav_item_home">Home</string>
    <string name="nav_item_friends">Friends</string>
    <string name="nav_item_notifications">Messages</string>
 
    <!-- navigation drawer item labels  -->
    <string-array name="nav_drawer_labels">
        <item>@string/nav_item_home</item>
        <item>@string/nav_item_friends</item>
        <item>@string/nav_item_notifications</item>
    </string-array>
 
    <string name="title_messages">Messages</string>
    <string name="title_friends">Friends</string>
    <string name="title_home">Home</string>
</resources>  

3. Open res ⇒ values ⇒ colors.xml and add the below color values. If you don’t find colors.xml, create a new resource file with the name.
1.打开res->values->colors 添加以下颜色,如果你没有找到colors.xml文件，那么新建一个.


colors.xml  
<?xml version="1.0" encoding="utf-8"?>
<resources>  

    <color name="colorPrimary">#F50057</color>
    <color name="colorPrimaryDark">#C51162</color>
    <color name="textColorPrimary">#FFFFFF</color>
    <color name="windowBackground">#FFFFFF</color>
    <color name="navigationBarColor">#000000</color>
    <color name="colorAccent">#FF80AB</color>
    
</resources>  

4. Open res ⇒ values ⇒ dimens.xml and add below dimensions.  
 开打dimens.xml文件，加入以下代码
 
 dimens.xml
<resources>  

    <!-- Default screen margins, per the Android Design guidelines. -->
    <dimen name="activity_horizontal_margin">16dp</dimen>
    <dimen name="activity_vertical_margin">16dp</dimen>
    <dimen name="nav_drawer_width">260dp</dimen> 
    
</resources>

5. Open styles.xml under res ⇒ values and add below styles. The styles defined in this styles.xml are common to all the android versions. Here I am naming my theme as MyMaterialTheme.
1. 开打styles.xml文件加入以下代码，在这里定义的style对于所有的androidbanben都是通用的。

styles.xml
<resources>
 
    <style name="MyMaterialTheme" parent="MyMaterialTheme.Base">
 
    </style>
 
    <style name="MyMaterialTheme.Base" 				parent="Theme.AppCompat.Light.DarkActionBar">
        <item name="android:windowNoTitle">true</item>
        <item name="windowActionBar">false</item>
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>
     
</resources>

1. Now under res, create a folder named values-v21. Inside values-v21, create another styles.xml with the below styles. These styles are specific to Android Lollipop only.

在res目录下，创建一个values-v21目录,并styles.xml
文件夹,这些style仅仅用于Android Lollipop

styles.xml
<resources>
 
    <style name="MyMaterialTheme" parent="MyMaterialTheme.Base">
        <item name="android:windowContentTransitions">true</item>
        <item name="android:windowAllowEnterTransitionOverlap">true</item>
        <item   name="android:windowAllowReturnTransitionOverlap">true</item>
        <item name="android:windowSharedElementEnterTransition">@android:transition/move</item>
        <item name="android:windowSharedElementExitTransition">@android:transition/move</item>
    </style>
 
</resources>

1. Now we have the basic Material Design styles ready. In order to apply the theme, open AndroidManifest.xml and modify the android:theme attribute of <application> tag.
2. 现在我们已经准备好Material Design style了， 为了使用这主题, 在AndroidManifest.xml文件中修改application theme属性如下：

AndroidManifest.xml  
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="androidhive.info.materialdesign" >
 
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/MyMaterialTheme" >
        <activity
            android:name=".activity.MainActivity"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
 
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
 
</manifest>

Now if you run the app, you can see the notification bar color changed to the color that we have mentioned in our styles.

现在如果你运行app，你能看到notification的颜色已经变成我们在style中设置的颜色.
<div class="image"> <img src="http://cdn4.androidhive.info/wp-content/uploads/2015/04/android-material-design-notification-bar.png?524b4b" alt="android-material-design-notification-bar" width="720px" height="auto" class="alignnone size-full wp-image-38182"></div>

###Adding the Toolbar (Action Bar)
添加toolbar(action bar)
Adding the toolbar is very easy. All you have to do is, create a separate layout for the toolbar and include it in other layout wherever you want the toolbar to be displayed.  
添加toolbar很简单,你要做的只是为toolbar另外创建一个layout,然后你想在哪里显示它，就在那个页面布局中include它

Create an xml file named toolbar.xml under res ⇒ layout and add android.support.v7.widget.Toolbar element. This create the toolbar with specific height and theming.  
在layout目录下创建toolbar.xml, 在里面添加android.support.v7.widget.Toolbar，并设置宽高.  


Open the layout file of your main activity (activity_main.xml) and add the toolbar using <include/> tag.

打开你的main activiy的布局,然后包含toolbar  

activity_main.xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
 
    <LinearLayout
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:orientation="vertical">
 
        <include
            android:id="@+id/toolbar"
            layout="@layout/toolbar" />
    </LinearLayout>
 
 
</RelativeLayout>  

Run the app and see if the toolbar displayed on the screen or not.  
运行app，确认toolbar是否已经显示了。

<div class="image"> <img src="http://cdn4.androidhive.info/wp-content/uploads/2015/04/android-material-design-toolbar1.png?524b4b" alt="android-material-design-toolbar" width="720px" height="auto" class="alignnone size-full wp-image-38187"></div>  

Now let’s try to add a toolbar title and enable the action items.
现在让我尝试添加toolbard额标题，并启用它的行为。  

11. Download this search icon and import it into Android Studio as a Image Asset.  
下载相关图片资源，然后导入到AS当中作为Image Asset.

12. To import the Image Asset in Android Studio, right click on res ⇒ New ⇒ Image Asset. It will show you a popup window to import the resource. Browse the search icon that you have downloaded in the above step, select Action Bar and Tab Icons for Asset Type and give the resource name as ic_search_action and proceed.

导入Image asset步骤,右键res目录->new->Image Asset 之后显示弹窗，然后找到你已经下载好的图片,选择Action Bar 和 Tab Icons， 给资源命名为ic_search_action. 如下图 

<div class="image"> <img src="http://cdn3.androidhive.info/wp-content/uploads/2015/04/android-studio-importing-image-asset.png?524b4b" alt="android-studio-importing-image-asset" width="720px" height="auto" class="alignnone size-full wp-image-38190"></div>  

Once the icon is imported, open menu_main.xml located under res ⇒ menu and add the search menu item as mentioned below.  
一旦图片导入了，打开menu_main.xml 提那件seach menu item  

menu_main.xml
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    tools:context=".MainActivity">
 
    <item
        android:id="@+id/action_search"
        android:title="@string/action_search"
        android:orderInCategory="100"
        android:icon="@drawable/ic_action_search"
        app:showAsAction="ifRoom" />
 
    <item
        android:id="@+id/action_settings"
        android:title="@string/action_settings"
        android:orderInCategory="100"
        app:showAsAction="never" />
</menu>  

Now open your MainActivity.java and do the below changes.  
打开MainActivity.java做以下修改.
> Extend the activity from ActionBarActivity  
MainActivity应从ACtionBarAcitiv继承

> Enable the toolbar by calling setSupportActionBar() by passing the toolbar object  
> 调用setSupportActionBar()来启用toolbar

> Override onCreateOptionsMenu() and onOptionsItemSelected() methods to enable toolbar action items.  
> 复写onCreateOptionsMenu() 和 onOptionsItemSelected()来启动toolbar菜单子目录的行为。  


 
    private Toolbar mToolbar;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        mToolbar = (Toolbar) findViewById(R.id.toolbar);
 
        setSupportActionBar(mToolbar);
        getSupportActionBar().setDisplayShowHomeEnabled(true);
    }
 
 
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }
 
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();
 
        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            return true;
        }
 
        return super.onOptionsItemSelected(item);
    }
  
  
After doing the above changes, if you run the app, you should see the search icon and action overflow icon.  
添加以上代码后，运行APP，你应该能够看到如下图所示的效果  

<div class="image"> <img src="http://cdn1.androidhive.info/wp-content/uploads/2015/04/android-material-design-toolbar-action-items.png?524b4b" alt="android-material-design-toolbar-action-items" width="720px" height="auto" class="alignnone size-full wp-image-38195"></div>  

Adding Navigation Drawer
添加Navigation Drawer  

Adding navigation drawer is same as that we do before lollipop, but instead if using ListView for menu items, we use RecyclerView in material design. So let’s see how to implement the navigation drawer with RecyclerView.  
添加Natigation Drawer的方式跟lollipop以前一样,但我们使用RecyclerView来实现Menu Items.  

在build.gradle中添加以下依赖库  

build.gradle  
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:22.0.0'
    compile 'com.android.support:recyclerview-v7:21.0.+'
}

添加NavDrawerItem.java文件作为menuItem  

 
 
    public NavDrawerItem() {
 
    }
 
    public NavDrawerItem(boolean showNotify, String title) {
        this.showNotify = showNotify;
        this.title = title;
    }
 
    public boolean isShowNotify() {
        return showNotify;
    }
 
    public void setShowNotify(boolean showNotify) {
        this.showNotify = showNotify;
    }
 
    public String getTitle() {
        return title;
    }
 
    public void setTitle(String title) {
        this.title = title;
    }  
    
Under res ⇒ layout, create an xml layout named nav_drawer_row.xml and add the below code. The layout renders each row in navigation drawer menu. If you want to customize the navigation drawer menu item, you have to do the changes in this file. For now it has only one TextView.  

添加nav_drawer_row.xml布局文件  
<table border="0" cellpadding="0" cellspacing="0"><caption>nav_drawer_row.xml</caption><tbody><tr><td class="code"><div class="container"><div class="line number1 index0 alt2"><code class="xml plain">&lt;?</code><code class="xml keyword">xml</code> <code class="xml color1">version</code><code class="xml plain">=</code><code class="xml string">"1.0"</code> <code class="xml color1">encoding</code><code class="xml plain">=</code><code class="xml string">"utf-8"</code><code class="xml plain">?&gt;</code></div><div class="line number2 index1 alt1"><code class="xml plain">&lt;</code><code class="xml keyword">RelativeLayout</code> <code class="xml color1">xmlns:android</code><code class="xml plain">=</code><code class="xml string">"<a href="http://schemas.android.com/apk/res/android">http://schemas.android.com/apk/res/android</a>"</code></div><div class="line number3 index2 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_width</code><code class="xml plain">=</code><code class="xml string">"match_parent"</code></div><div class="line number4 index3 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_height</code><code class="xml plain">=</code><code class="xml string">"wrap_content"</code></div><div class="line number5 index4 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:clickable</code><code class="xml plain">=</code><code class="xml string">"true"</code><code class="xml plain">&gt;</code></div><div class="line number6 index5 alt1">&nbsp;</div><div class="line number7 index6 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml plain">&lt;</code><code class="xml keyword">TextView</code></div><div class="line number8 index7 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:id</code><code class="xml plain">=</code><code class="xml string">"@+id/title"</code></div><div class="line number9 index8 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_width</code><code class="xml plain">=</code><code class="xml string">"fill_parent"</code></div><div class="line number10 index9 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_height</code><code class="xml plain">=</code><code class="xml string">"wrap_content"</code></div><div class="line number11 index10 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:paddingLeft</code><code class="xml plain">=</code><code class="xml string">"30dp"</code></div><div class="line number12 index11 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:paddingTop</code><code class="xml plain">=</code><code class="xml string">"10dp"</code></div><div class="line number13 index12 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:paddingBottom</code><code class="xml plain">=</code><code class="xml string">"10dp"</code></div><div class="line number14 index13 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:textSize</code><code class="xml plain">=</code><code class="xml string">"15dp"</code></div><div class="line number15 index14 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:textStyle</code><code class="xml plain">=</code><code class="xml string">"bold"</code> <code class="xml plain">/&gt;</code></div><div class="line number16 index15 alt1">&nbsp;</div><div class="line number17 index16 alt2"><code class="xml plain">&lt;/</code><code class="xml keyword">RelativeLayout</code><code class="xml plain">&gt;</code></div></div></td></tr></tbody></table>  

Download this profile icon and paste it in your drawable folder. This step is optional, but this icon used in the navigation drawer header part.  
下载这个页面的<a href="http://api.androidhive.info/images/ic_profile.png">图片</a>,这图片用在导航栏的头部  

13. Create another xml layout named fragment_navigation_drawer.xml and add the below code. This layout renders the complete navigation drawer view. This layout contains a header section to display the user information and a RecyclerView to display the list view.  
创建另一个布局文件fragment_navigation_drawer.xml,如下代码:  
<table border="0" cellpadding="0" cellspacing="0"><caption>fragment_navigation_drawer.xml</caption><tbody><tr><td class="code"><div class="container"><div class="line number1 index0 alt2"><code class="xml plain">&lt;</code><code class="xml keyword">RelativeLayout</code> <code class="xml color1">xmlns:android</code><code class="xml plain">=</code><code class="xml string">"<a href="http://schemas.android.com/apk/res/android">http://schemas.android.com/apk/res/android</a>"</code></div><div class="line number2 index1 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_width</code><code class="xml plain">=</code><code class="xml string">"match_parent"</code></div><div class="line number3 index2 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_height</code><code class="xml plain">=</code><code class="xml string">"match_parent"</code></div><div class="line number4 index3 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:background</code><code class="xml plain">=</code><code class="xml string">"@android:color/white"</code><code class="xml plain">&gt;</code></div><div class="line number5 index4 alt2">&nbsp;</div><div class="line number6 index5 alt1">&nbsp;</div><div class="line number7 index6 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml plain">&lt;</code><code class="xml keyword">RelativeLayout</code></div><div class="line number8 index7 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:id</code><code class="xml plain">=</code><code class="xml string">"@+id/nav_header_container"</code></div><div class="line number9 index8 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_width</code><code class="xml plain">=</code><code class="xml string">"match_parent"</code></div><div class="line number10 index9 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_height</code><code class="xml plain">=</code><code class="xml string">"140dp"</code></div><div class="line number11 index10 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_alignParentTop</code><code class="xml plain">=</code><code class="xml string">"true"</code></div><div class="line number12 index11 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:background</code><code class="xml plain">=</code><code class="xml string">"@color/colorPrimary"</code><code class="xml plain">&gt;</code></div><div class="line number13 index12 alt2">&nbsp;</div><div class="line number14 index13 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml plain">&lt;</code><code class="xml keyword">ImageView</code></div><div class="line number15 index14 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_width</code><code class="xml plain">=</code><code class="xml string">"70dp"</code></div><div class="line number16 index15 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_height</code><code class="xml plain">=</code><code class="xml string">"70dp"</code></div><div class="line number17 index16 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:src</code><code class="xml plain">=</code><code class="xml string">"@drawable/ic_profile"</code></div><div class="line number18 index17 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:scaleType</code><code class="xml plain">=</code><code class="xml string">"fitCenter"</code></div><div class="line number19 index18 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_centerInParent</code><code class="xml plain">=</code><code class="xml string">"true"</code> <code class="xml plain">/&gt;</code></div><div class="line number20 index19 alt1">&nbsp;</div><div class="line number21 index20 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml plain">&lt;/</code><code class="xml keyword">RelativeLayout</code><code class="xml plain">&gt;</code></div><div class="line number22 index21 alt1">&nbsp;</div><div class="line number23 index22 alt2">&nbsp;</div><div class="line number24 index23 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml plain">&lt;</code><code class="xml keyword">android.support.v7.widget.RecyclerView</code></div><div class="line number25 index24 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:id</code><code class="xml plain">=</code><code class="xml string">"@+id/drawerList"</code></div><div class="line number26 index25 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_width</code><code class="xml plain">=</code><code class="xml string">"match_parent"</code></div><div class="line number27 index26 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_height</code><code class="xml plain">=</code><code class="xml string">"wrap_content"</code></div><div class="line number28 index27 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_below</code><code class="xml plain">=</code><code class="xml string">"@id/nav_header_container"</code></div><div class="line number29 index28 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_marginTop</code><code class="xml plain">=</code><code class="xml string">"15dp"</code> <code class="xml plain">/&gt;</code></div><div class="line number30 index29 alt1">&nbsp;</div><div class="line number31 index30 alt2">&nbsp;</div><div class="line number32 index31 alt1"><code class="xml plain">&lt;/</code><code class="xml keyword">RelativeLayout</code><code class="xml plain">&gt;</code></div></div></td></tr></tbody></table>  

As the RecyclerView is customized, we need an adapter class to render the custom xml layout. So under adapter package, create a class named NavigationDrawerAdapter.java and paste the below code. This adapter class inflates nav_drawer_row.xml and renders the RecycleView drawer menu.  
由于RecyclerView是自定义的，我们需要创建一个Adapter来渲染自定的布局, 因此创建NavigationDrawerAdapter.java文件  
 
    public NavigationDrawerAdapter(Context context, List<NavDrawerItem> data) {
        this.context = context;
        inflater = LayoutInflater.from(context);
        this.data = data;
    }
 
    public void delete(int position) {
        data.remove(position);
        notifyItemRemoved(position);
    }
 
    @Override
    public MyViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View view = inflater.inflate(R.layout.nav_drawer_row, parent, false);
        MyViewHolder holder = new MyViewHolder(view);
        return holder;
    }
 
    @Override
    public void onBindViewHolder(MyViewHolder holder, int position) {
        NavDrawerItem current = data.get(position);
        holder.title.setText(current.getTitle());
    }
 
    @Override
    public int getItemCount() {
        return data.size();
    }
 
    class MyViewHolder extends RecyclerView.ViewHolder {
        TextView title;
 
        public MyViewHolder(View itemView) {
            super(itemView);
            title = (TextView) itemView.findViewById(R.id.title);
        }
    }
  
  
Under adapter package, create a fragment named FragmentDrawer.java. In Android Studio, to create a new fragment, Right click on adapter ⇒ New ⇒ Fragment ⇒ Fragment (Blank) and give your fragment class name.  

创建FragmentDrawer.java  
 
    private RecyclerView recyclerView;
    private ActionBarDrawerToggle mDrawerToggle;
    private DrawerLayout mDrawerLayout;
    private NavigationDrawerAdapter adapter;
    private View containerView;
    private static String[] titles = null;
    private FragmentDrawerListener drawerListener;
 
    public FragmentDrawer() {
 
    }
 
    public void setDrawerListener(FragmentDrawerListener listener) {
        this.drawerListener = listener;
    }
 
    public static List<NavDrawerItem> getData() {
        List<NavDrawerItem> data = new ArrayList<>();
 
 
        // preparing navigation drawer items
        for (int i = 0; i < titles.length; i++) {
            NavDrawerItem navItem = new NavDrawerItem();
            navItem.setTitle(titles[i]);
            data.add(navItem);
        }
        return data;
    }
 
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
 
        // drawer labels
        titles = getActivity().getResources().getStringArray(R.array.nav_drawer_labels);
    }
 
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Inflating view layout
        View layout = inflater.inflate(R.layout.fragment_navigation_drawer, container, false);
        recyclerView = (RecyclerView) layout.findViewById(R.id.drawerList);
 
        adapter = new NavigationDrawerAdapter(getActivity(), getData());
        recyclerView.setAdapter(adapter);
        recyclerView.setLayoutManager(new LinearLayoutManager(getActivity()));
        recyclerView.addOnItemTouchListener(new RecyclerTouchListener(getActivity(), recyclerView, new ClickListener() {
            @Override
            public void onClick(View view, int position) {
                drawerListener.onDrawerItemSelected(view, position);
                mDrawerLayout.closeDrawer(containerView);
            }
 
            @Override
            public void onLongClick(View view, int position) {
 
            }
        }));
 
        return layout;
    }
 
 
    public void setUp(int fragmentId, DrawerLayout drawerLayout, final Toolbar toolbar) {
        containerView = getActivity().findViewById(fragmentId);
        mDrawerLayout = drawerLayout;
        mDrawerToggle = new ActionBarDrawerToggle(getActivity(), drawerLayout, toolbar, R.string.drawer_open, R.string.drawer_close) {
            @Override
            public void onDrawerOpened(View drawerView) {
                super.onDrawerOpened(drawerView);
                getActivity().invalidateOptionsMenu();
            }
 
            @Override
            public void onDrawerClosed(View drawerView) {
                super.onDrawerClosed(drawerView);
                getActivity().invalidateOptionsMenu();
            }
 
            @Override
            public void onDrawerSlide(View drawerView, float slideOffset) {
                super.onDrawerSlide(drawerView, slideOffset);
                toolbar.setAlpha(1 - slideOffset / 2);
            }
        };
 
        mDrawerLayout.setDrawerListener(mDrawerToggle);
        mDrawerLayout.post(new Runnable() {
            @Override
            public void run() {
                mDrawerToggle.syncState();
            }
        });
 
    }
 
    public static interface ClickListener {
        public void onClick(View view, int position);
 
        public void onLongClick(View view, int position);
    }
 
    static class RecyclerTouchListener implements RecyclerView.OnItemTouchListener {
 
        private GestureDetector gestureDetector;
        private ClickListener clickListener;
 
        public RecyclerTouchListener(Context context, final RecyclerView recyclerView, final ClickListener clickListener) {
            this.clickListener = clickListener;
            gestureDetector = new GestureDetector(context, new GestureDetector.SimpleOnGestureListener() {
                @Override
                public boolean onSingleTapUp(MotionEvent e) {
                    return true;
                }
 
                @Override
                public void onLongPress(MotionEvent e) {
                    View child = recyclerView.findChildViewUnder(e.getX(), e.getY());
                    if (child != null && clickListener != null) {
                        clickListener.onLongClick(child, recyclerView.getChildPosition(child));
                    }
                }
            });
        }
 
        @Override
        public boolean onInterceptTouchEvent(RecyclerView rv, MotionEvent e) {
 
            View child = rv.findChildViewUnder(e.getX(), e.getY());
            if (child != null && clickListener != null && gestureDetector.onTouchEvent(e)) {
                clickListener.onClick(child, rv.getChildPosition(child));
            }
            return false;
        }
 
        @Override
        public void onTouchEvent(RecyclerView rv, MotionEvent e) {
        }
    }
 
    public interface FragmentDrawerListener {
        public void onDrawerItemSelected(View view, int position);
    }
   
Finally open main activity layout (activity_main.xml) and modify the layout as below. In this layout we are adding android.support.v4.widget.DrawerLayout to display the navigation drawer menu.

Also you have to give the correct path of your FragmentDrawer in <fragment> element.  
最后在activity_main.xml中添加DrawerLayout，如下代码  
根布局是DrawerLayout
 
 <android.support.v4.widget.DrawerLayout
 android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
 
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
 
        <LinearLayout
            android:id="@+id/container_toolbar"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical">
 
            <include
                android:id="@+id/toolbar"
                layout="@layout/toolbar" />
        </LinearLayout>
 
        <FrameLayout
            android:id="@+id/container_body"
            android:layout_width="fill_parent"
            android:layout_height="0dp"
            android:layout_weight="1" />
 
 
    </LinearLayout>
 
 
    <fragment
        android:id="@+id/fragment_navigation_drawer"
        android:name="androidhive.info.materialdesign.activity.FragmentDrawer"
        android:layout_width="@dimen/nav_drawer_width"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        app:layout="@layout/fragment_navigation_drawer"
        tools:layout="@layout/fragment_navigation_drawer" />
        
    <android.support.v4.widget.DrawerLayout>  
   Now we have all the layout files and java classes ready in place. Let’s do the necessary changes in MainActivity to make the navigation drawer functioning.  
   现在我们已经完成了所有的的布局和jav文件,接下来我们要实现navigation draw而得功能.  
   
   Open your MainActivity.java and do the below changes.

> Implement the activity from FragmentDrawer.FragmentDrawerListener and add the onDrawerItemSelected() override method.

> Create an instance of FragmentDrawer and set the drawer selected listeners.  
MainActivity实现ragmentDrawer.FragmentDrawerListener 如下代码：  
private Toolbar mToolbar;
    private FragmentDrawer drawerFragment;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        mToolbar = (Toolbar) findViewById(R.id.toolbar);
 
        setSupportActionBar(mToolbar);
        getSupportActionBar().setDisplayShowHomeEnabled(true);
 
        drawerFragment = (FragmentDrawer)
                getSupportFragmentManager().findFragmentById(R.id.fragment_navigation_drawer);
        drawerFragment.setUp(R.id.fragment_navigation_drawer, (DrawerLayout) findViewById(R.id.drawer_layout), mToolbar);
        drawerFragment.setDrawerListener(this);
    }
 
 
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }
 
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();
 
        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            return true;
        }
 
        return super.onOptionsItemSelected(item);
    }
 
    @Override
    public void onDrawerItemSelected(View view, int position) {
 
    }  
    
    
Now if you run the app, you can see the navigation drawer with a header and few list items in it.  
现在运行APP能看到以下效果.  
<div class="image"> <img src="http://cdn1.androidhive.info/wp-content/uploads/2015/04/androd-material-design-navigation-drawer.png?524b4b" alt="androd-material-design-navigation-drawer" width="720px" height="auto" class="alignnone size-full wp-image-38198"></div>  

Implementing Navigation Drawer Item Selection
###实现Navigation Drawer的选择  
Although navigation drawer is functioning, you can see the selection of drawer list items not working. This is because we are yet to implement the click listener on RecyclerView items.
虽然navigation drawer已经能够工作了，但是MENU的子选项无法工作，这是因为我们还没有处理RecyclerView item是的点击监听。   

As we have three menu items in navigation drawer (Home, Friends & Messages), we need to create three separate fragment classes for each menu item.    
由于我们有三子菜单(Home,Friends&Mesages), 因此我们需要创建三个独立的Fragment  
首先创建Homed的布局  

<div id="highlighter_127860" class="syntaxhighlighter nogutter  xml"><table border="0" cellpadding="0" cellspacing="0"><caption>fragment_home.xml</caption><tbody><tr><td class="code"><div class="container"><div class="line number1 index0 alt2"><code class="xml plain">&lt;</code><code class="xml keyword">RelativeLayout</code> <code class="xml color1">xmlns:android</code><code class="xml plain">=</code><code class="xml string">"<a href="http://schemas.android.com/apk/res/android">http://schemas.android.com/apk/res/android</a>"</code></div><div class="line number2 index1 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">xmlns:tools</code><code class="xml plain">=</code><code class="xml string">"<a href="http://schemas.android.com/tools">http://schemas.android.com/tools</a>"</code></div><div class="line number3 index2 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_width</code><code class="xml plain">=</code><code class="xml string">"match_parent"</code></div><div class="line number4 index3 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_height</code><code class="xml plain">=</code><code class="xml string">"match_parent"</code></div><div class="line number5 index4 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:orientation</code><code class="xml plain">=</code><code class="xml string">"vertical"</code></div><div class="line number6 index5 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">tools:context</code><code class="xml plain">=</code><code class="xml string">"androidhive.info.materialdesign.activity.HomeFragment"</code><code class="xml plain">&gt;</code></div><div class="line number7 index6 alt2">&nbsp;</div><div class="line number8 index7 alt1">&nbsp;</div><div class="line number9 index8 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml plain">&lt;</code><code class="xml keyword">TextView</code></div><div class="line number10 index9 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:id</code><code class="xml plain">=</code><code class="xml string">"@+id/label"</code></div><div class="line number11 index10 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_alignParentTop</code><code class="xml plain">=</code><code class="xml string">"true"</code></div><div class="line number12 index11 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_marginTop</code><code class="xml plain">=</code><code class="xml string">"100dp"</code></div><div class="line number13 index12 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_width</code><code class="xml plain">=</code><code class="xml string">"fill_parent"</code></div><div class="line number14 index13 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_height</code><code class="xml plain">=</code><code class="xml string">"wrap_content"</code></div><div class="line number15 index14 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:gravity</code><code class="xml plain">=</code><code class="xml string">"center_horizontal"</code></div><div class="line number16 index15 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:textSize</code><code class="xml plain">=</code><code class="xml string">"45dp"</code></div><div class="line number17 index16 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:text</code><code class="xml plain">=</code><code class="xml string">"HOME"</code></div><div class="line number18 index17 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:textStyle</code><code class="xml plain">=</code><code class="xml string">"bold"</code><code class="xml plain">/&gt;</code></div><div class="line number19 index18 alt2">&nbsp;</div><div class="line number20 index19 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml plain">&lt;</code><code class="xml keyword">TextView</code></div><div class="line number21 index20 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_below</code><code class="xml plain">=</code><code class="xml string">"@id/label"</code></div><div class="line number22 index21 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_centerInParent</code><code class="xml plain">=</code><code class="xml string">"true"</code></div><div class="line number23 index22 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_width</code><code class="xml plain">=</code><code class="xml string">"fill_parent"</code></div><div class="line number24 index23 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_height</code><code class="xml plain">=</code><code class="xml string">"wrap_content"</code></div><div class="line number25 index24 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:textSize</code><code class="xml plain">=</code><code class="xml string">"12dp"</code></div><div class="line number26 index25 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:layout_marginTop</code><code class="xml plain">=</code><code class="xml string">"10dp"</code></div><div class="line number27 index26 alt2"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:gravity</code><code class="xml plain">=</code><code class="xml string">"center_horizontal"</code></div><div class="line number28 index27 alt1"><code class="xml spaces">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code class="xml color1">android:text</code><code class="xml plain">=</code><code class="xml string">"Edit fragment_home.xml to change the appearance"</code> <code class="xml plain">/&gt;</code></div><div class="line number29 index28 alt2">&nbsp;</div><div class="line number30 index29 alt1"><code class="xml plain">&lt;/</code><code class="xml keyword">RelativeLayout</code><code class="xml plain">&gt;</code></div></div></td></tr></tbody></table></div>  

创建HomeFragment.java文件 
 
    public HomeFragment() {
        // Required empty public constructor
    }
 
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
 
    }
 
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        View rootView = inflater.inflate(R.layout.fragment_home, container, false);
 
 
        // Inflate the layout for this fragment
        return rootView;
    }
 
    @Override
    public void onAttach(Activity activity) {
        super.onAttach(activity);
    }
 
    @Override
    public void onDetach() {
        super.onDetach();
    }
    
    
Create two more fragment classes named FriendsFragment.java, MessagesFragment.java and respected layout files named fragment_friends.xml and fragment_messages.xml and add the code from above two steps.
创建另外两个FRAGMENT，跟上面一样  

Now open MainActivity.java and do the below changes. In the below code
displayView() method displays the fragment view respected the navigation menu item selection. This method should be called in onDrawerItemSelected() to render the respected view when a navigation menu item is selected.  

对MainActivity.java做如下修改  

    private static String TAG = MainActivity.class.getSimpleName();
 
    private Toolbar mToolbar;
    private FragmentDrawer drawerFragment;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        mToolbar = (Toolbar) findViewById(R.id.toolbar);
 
        setSupportActionBar(mToolbar);
        getSupportActionBar().setDisplayShowHomeEnabled(true);
 
        drawerFragment = (FragmentDrawer)
                getSupportFragmentManager().findFragmentById(R.id.fragment_navigation_drawer);
        drawerFragment.setUp(R.id.fragment_navigation_drawer, (DrawerLayout) findViewById(R.id.drawer_layout), mToolbar);
        drawerFragment.setDrawerListener(this);
 
        // display the first navigation drawer view on app launch
        displayView(0);
    }
 
 
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }
 
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();
 
        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            return true;
        }
 
        if(id == R.id.action_search){
            Toast.makeText(getApplicationContext(), "Search action is selected!", Toast.LENGTH_SHORT).show();
            return true;
        }
 
        return super.onOptionsItemSelected(item);
    }
 
    @Override
    public void onDrawerItemSelected(View view, int position) {
            displayView(position);
    }
 
    private void displayView(int position) {
        Fragment fragment = null;
        String title = getString(R.string.app_name);
        switch (position) {
            case 0:
                fragment = new HomeFragment();
                title = getString(R.string.title_home);
                break;
            case 1:
                fragment = new FriendsFragment();
                title = getString(R.string.title_friends);
                break;
            case 2:
                fragment = new MessagesFragment();
                title = getString(R.string.title_messages);
                break;
            default:
                break;
        }
 
        if (fragment != null) {
            FragmentManager fragmentManager = getSupportFragmentManager();
            FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
            fragmentTransaction.replace(R.id.container_body, fragment);
            fragmentTransaction.commit();
 
            // set the toolbar title
            getSupportActionBar().setTitle(title);
        }
    }   
    
    
Now if you run the app, you can see the selection of navigation drawer menu is working and respected view displayed below the toolbar.

现在运行app,你能够选择menu的子菜单了。如下效果。

<div class="image"> <img src="http://cdn1.androidhive.info/wp-content/uploads/2015/04/android-material-design-navigation-drawer-1.png?524b4b" alt="android-material-design-navigation-drawer-1" width="720px" height="auto" class="alignnone size-full wp-image-38203"></div>   

<div class="image"><img src="http://cdn1.androidhive.info/wp-content/uploads/2015/04/android-material-design-navigation-drawer-2.png?524b4b" alt="android-material-design-navigation-drawer-2" width="720px" height="auto" class="alignnone size-full wp-image-38204"></div>  

<div class="image"><img src="http://cdn1.androidhive.info/wp-content/uploads/2015/04/android-material-design-navigation-drawer-3.png?524b4b" alt="android-material-design-navigation-drawer-3" width="720px" height="auto" class="alignnone size-full wp-image-38205"></div>