那些被遗忘的Matetial Design组件
---

> * 原文链接 : [Missing Android Material Components](http://www.dmytrodanylyk.com/pages/blog/missing-material-components.html)
* 原文作者 : [Dmytro Danylyk](http://www.dmytrodanylyk.com/index.html#/about)
* [译文出自 :  开发技术前线 www.devtf.cn](http://www.devtf.cn)
* 译者 : [wly2014](https://github.com/wly2014) 
* 校对者: [yinna317](https://github.com/yinna317)  
* 状态 :  未完成 / 校对中 / 完成 


介绍

Hello, my name is Dmytro Danylyk, I am developing Android Applications for about 4 years since G1 was released. I am finalist of Google Apps Developer Challenge 2012. I always try to help people on StackOverflow (8 000 reputation points. You may know me from my technical articles or open source libraries, like Process Buttons and Circular Progress Button, maybe some even used them.

Today I want to speak about Missing Android Material Components, which basically means Android Material Components which are present in Google Material Spec, but missing in Android SDK and AppCompat v7 library. Also I will share some gists and tutorials, recipes in general, of how to make those components.

你好，我是Dmytro Danylyk，自从G1发布以来，我开发Android应用已经有4年的时间了。我还是Google Apps Developer Challenge 2012 的决赛选手。我一直试着在StackOverflow（8000个声望点了）上帮助他人。你可能通过我的技术文章或是开源库了解过我，如Process Button 和Circular Progress Button ，而且很有可能你正在使用它们。

今天我想说的是：缺失的Android Material组件，主要是指那些在Google Material Spec中提到的，但Android SDK和AppCompat v7库中又没有的Android Material组件。另外还会分享一些gists和教程，以及制作这些组件诀窍。

But first, I would like to answer a question, which you will probably ask me.

但是首先回答一个问题，你可能会这样问：

Why we should write material components ourselves and not use 3rd part libraries?

为什么我们自己要写Material组件，而不是使用第三方的库？

My answer is - you should always write something yourself, if you have time for this, because it’s safer. Add new feature or fix bug in your own code - always easier.

Here`s how things are done in my projects. I have a library module called ‘material-components’ where I put all my custom material components. For my next project I will just copy / paste this module.

我的答案是：你应该自己写一些东西，如果你有时间的话，因为这样才稳妥。加入新的特性和修改bug才会变得更容易。

下面是我在我的工程中做的一些事情。我有一个库叫做“material-components”，我放了些我自定义的Material组件在里面。这样在我的下一个工程里就可以复制粘贴其中的代码了。

Slide 1

Now let’s get started. As you know Google has released AppCompat v7 library, where we have port of 8 material widgets. Google also released spec about material design for 50+ pages. Fair enough. I think Google want to say something to us:

- Here is spec, do it yourself.

That's exactly what we are going to do.

Slide 1

现在我们开始吧。如你所知，Google已经发布了AppCompat v7库了，里面有8个Material组件。Google也已经发布了超过50页的关于材料设计的规范。可以说，Google想告诉我们：

- 这只是规范，具体的要自己来做。

这也正是我们将要做的。

Slide 2

To start cooking we need to prepare Ingredients which will be included almost in all our material component.

Slide 2

开始之前，我们需要准备要素，将会应用在我们的Material组件中。

Slide 3

AppCompat v7 library not just contains port of some material widgets, it contains a lot of predefined material colors, text sizes and dimensions. Our first ingredient is - Typography which in Material Design has changed.

Slide 3/1

Here you can see sample code of AppCompat.TextAppearance - use them where possible.

Slide 3

AppCompat v7库并不是仅仅包括一些Material的组件，它也包括大量的预定义的Material 色彩，字体大小和尺寸。我们的第一个要素就是-Typography，它在材料设计中已经被更改过了。

Slide 3/1

其中，你能看到一些Appcompat.TextAppearance 的示例代码,尽可能的使用它们。

Slide 4

Selector - is another important thing which has changed in Material Design. With release of Android Lollipop we now have Ripple Drawable which is used as a selector for all widgets. Unfortunately due to performance issue Ripple Drawable is only available for Android Lollipop. To make selectors look more ‘material like’ I suggest to use Fade Selector.

Slide 4/1

So recipe for selector for Android Lollipop is a Ripple Drawable.

Slide 4/2

And for older versions - Fade Selector. To create Fade Selector you only need to set ‘exitFadeDuration’ and ‘enterFadeDuration’ attributes for any of your selectors.

Slide 4

Selector-是另一个重要的材料设计中做出改变的东西。随着Android Lollipop的发布，我们有了Ripple Drawable，可以作为任何组件的选择器。但不幸的是，由于表现的问题，Ripple Drawable只能在Android Lollipop上使用。为了让选择器看起来更“Material”，我建议使用Fade Selector。

Slide 4/1

Android Lollipop的选择器的秘诀就是Ripple Drawable。

Slide 4/2

对于更老的版本是-Fade Selector。为了创建Fade Selector，你只需要为每一个selector设置“exitFadeDuration”和“enterFadeDuration”参数。

Slide 5

The last ingredient which we will need for today is - Shadow. Since objects are placed on top of each other, they have different shadow. We all know in Android Lollipop we have elevation attribute, but what to do for previous versions? I prepared three different recipes, each have their own pros and cons.

Slide 5

我们需要的最后一个要素是-Shadow。因为对象被放置在彼此之上，所以它们会产生不同的阴影。我们都知道，在Android Lollipop中有elevation（高度）的参数，但是在之前的版本中该怎么做呐？我准备了三个不同的秘诀，每一个都有自己的优缺点。

Slide 5/1

As you may know Google released CardView library which has cardElevation attribute. This view perfectly draw shadow and uses three different algorithms based on Android version it’s running. However there are some things which are missing:

* CardView can't draw circle shadow.
* Not possible to set shadow position.
* Not possible to set shadow color. (Make shadow darker or lighter).
* No method to set selector for shadow. (Lift on touch).

Slide 5/1

如你所知，Google发布了CardView库，它包含了cardElevation的属性。这些view完美的表现出阴影，并且基于Android的运行版本而采用三个不同的算法。

* CardView不能够画圆形阴影。
* 不支持设置阴影的位置。
* 不支持设置阴影的颜色。（让阴影更亮或更暗）
* 不能设置阴影选择器。（Lift ontouch）

Slide 5/2

Second recipe is to use 9.png image as a background. This is a good approach in terms of performance, but:

* For every component you need shadow image for all drawable-dpi folders.
* Ton's of resources increase apk size.
* Not flexible for changes.

Slide 5/2

第二个秘诀就是使用9.png图片作为背景。就表现而言，这是一个比较好的方法，但是：

* 对于每一个组件，你要准备全部的drawable-dpi文件夹中的阴影图片。
* 大量的资源将增加应用的大小
* 改变起来不灵活

Slide 5/3

Third, and my favorite recipe is a custom ShadowLayout which can draw shadow. The reason I made it was to cover all issues of CardView. As you can see you can set shadowRadius and offset to get different type of shadow.

Slide 5/3

第三，我最喜欢的秘诀是自定义ShadowLayout，它可以实现阴影。制作它的原因就是要解决掉CardView的问题。如你所见，你可以设置shadowRadius和offset来得到不同效果的阴影。

Slide 5/4

With current version you can set shadow color, offset, radius, and corner radius. If you want a circle shadow, just set corner radius the same value as your view size.

Slide 5/4

在当前的版本中，你可以设置阴影的颜色，偏移量，半径和圆角半径。如果你想要一个圆形的阴影，只需要设置圆角半径为你的view半径的大小。

Slide 5/5

To draw shadow I am using setShadowLayer method of Paint object. There are two approaches how ShadowLayout can be made.

* First is to create Bitmap, draw shadow on it, then set this Bitmap as a background of layout.
* Second to draw shadow on layout canvas (without hardware acceleration)

The layout itself is very simple, only around 100 lines of code. But this doesn't mean it is very efficient. Generating Bitmap or drawing on Canvas without hardware acceleration will cost you some memory.

Slide 5/6

You can find source code and tutorial of ShadowLayout by following this links.

Slide 5/5

为了画出阴影，我使用Paint对象的setShadowLayer方法。这有两种创建ShadowLayout的方法：

* 第一种就是创建一个Bitmap，在上面显示阴影，然后将这个Bitmap设置成Layout的背景。
* 第二种是在Layout的canvas上画出阴影（禁用硬加速）。

这种Layout本身很简单只需要100行左右的代码。但是这并不意味着高效。禁用硬加速时，生成Bitmap或者在Canvas上画会占用大量的内存。

Slide 5/6

你可以通过下面的链接找到ShadowLayout的源代码与教程。

Slide 6

It’s time to start making some simple stuff.

Slide 6

是时候展现真正的案例了。

Slide 7

Of course we will start from buttons. In material design we have three types of buttons: Floating Action, Raised and Flat. None of this are available in AppCompat library.

Slide 7/1

Flat button is the simplest.

Slide 7/2

It can be made with a simple style. As a background we will use selector, prepared before. Other stuff you can take from Material Design Spec.

Slide 7/3

Raised Button is little bit more complicated, because it has shadow.

Slide 7

当然，我们将从Button开始。在材料设计中，我们有三种Button：Floating Action，Raised 和 Flat。但AppCompat库中都没有。

Slide 7/1

Flat button是最简单的。

Slide 7/2

它可以有一个最简单的样式。作为背景，我们将使用之前准备好的selector，其他的东西你可以从材料设计规范中得到。

Slide 7/3

Raised Button有点复杂，因为它有阴影。

Slide 7/4

Recipe for this button is to wrap Button with CardView and set cardElevation to imitate shadow for pre Lollipop devices.

Slide 7/5

Floating Action Button brings another issue - circle shadow.

Slide 7/6

My recipe for this kind of button is to wrap ImageButton with ShadowLayout, set cornerRadius attribute to the same value as size of FAB to make circle shadow.

Slide 7/7

Now answers to some questions you may have. Again, why we didn't use CardView to make shadow for FAB is because we will get some extra outlines.

Slide 7/8

Why we didn't use .png images? This is most efficient way to draw shadow, but it require set of resources.

Slide 7/9

I prepared tutorials and gists of how to make those buttons, but some are still in progress.

Slide 7/4

这种button的秘诀就是使用CardView包裹起来，并设置cardElevation属性来在Lollipop之前的设备上模拟阴影的效果。

Slide 7/5

Floating Action Button（FAB）有个问题-圆形的阴影。

Slide 7/6

对付这类的Button，我的秘诀就是使用ShadowLayout包裹一个ImageButton，并设置cornerRadious参数为FAB的大小来制作圆形的阴影。

Slide 7/7

现在，一些问题的答案已经有了。再问一次，为什么我们不用CardView来为FAB制作阴影，因为这将会带额外的轮廓。

Slide 7/8

为什么我们不使用.png图片呢？这是画阴影的最有效的方法，但是它需要大量的资源。

Slide 7/9

我也准备了关于如何制作这些button的教程和gists，但是有些还未完成。

Slide 8

The next thing which we are missing are error labels for EditText.

Slide 8/1

Recipe is pretty simple - custom view group which will wrap EditText, show error label and tint EditText background to red or any other color.

Slide 8/2

However there are some remarks.

* To tint EditText drawable you can use ColorFilter.
* EditText background Drawable is not tinted, until EditText remains focus, don't forget to switch focus to other view.
* For me, implementation of this view toked ~ 100 lines of code so keep it simple
Slide 8/3

You can check out tutorials and gist following this links.

Slide 8

下一个我们缺失的东西是EditText的错误标签。

Slide 8/1

秘诀很简单-包裹EditText的自定义的viewgroup，来展现错误标签和将背景着色成红色或者其他颜色。

Slide 8/2

然而还有些注意事项。
	你可以使用ColorFilter来着色EditText 的drawable。
	直到获得焦点，EditText的背景Drawable才能被着色，而且还要记得切换焦点到其他的view。
	对我来说，实现这种效果也就100行代码，所以要简单点。

Slide 8/3

你可以在链接中找到教程和gist。

Slide 9

Floating label is something similar to error labels, but it appears with animation on top instead of bottom, also we don't need to deal with tinting EditText background.

Slide 9/1

Recipe is also similar - custom view group which will wrap EditText, show floating label when EditText is in focus.

Slide 9/2

Chris Banes prepared great tutorial and gist.

Slide 9

Floating label与错误标签相似，但是它以动画的形式出现在顶部而不是底部，并且也不需要着色EditText的背景。

Slide 9/1

秘诀也是相似的-包裹EditText的自定义的view group，当EditText获得焦点时，悬浮标签显示。

Slide 9/2

Chris Banes 准备了很好的教程和gist。

Slide 10

Material dialogs are only available in sdk v21, so to make them look consistent we need to create our own Material Dialog .

Slide 10/1

Recipe is the following - extend Dialog class and set custom content view. As a root layout use either CardView or ShadowLayout . Since Material Dialog can display only title or title and content, can have one, two or three buttons position in different places, it must have several configurable settings. To achieve this you can use Builder pattern as in sample code.

Slide 10

Material的对话框仅仅在sdk v21上可用，所以要想让他们看起来，我们需要创建自己的Material Dialog。

Slide 10/1

秘诀如下-继承自Dialog类，并设置自定义的内容view。使用CardView或者ShadowLayout作为根布局。因为Material Dialog只能显示标题或者标题和内容，能够有一个，两个或者三个Button分布在不同的地方，它必须有数个设置。为了实现这种效果，你可以在示例代码中使用Builder模式。

Slide 10/2

Remarks

As a root view preferable to use ShadowLayout which gives you darker shadow in comparison with CardView.
Material Dialog use lighter dim amount value.
For buttons use AppCompat.Button.Flat style (see Flat button section of this presentation).
For text style use TextAppearance.AppCompat.Body1 and TextAppearance.AppCompat.Title from appcompat-v7 library.
Slide 10/3

Unfortunately tutorial and gist are still in progress :)

Slide 10/2

注意：
	作为根view，推荐使用ShadowLayout，相比于CardView它能提供更暗的阴影。
	Material Dialog使用较浅的暗度。
	对于Button，使用AppCompat.Button.Flat样式（参见Flat Button节）
	对于文字样式，使用AppCompat-v7库中的TextAppearance.AppCompat.Body1和TextAppearance.AppCompat.Title

Slide 10/3

很抱歉，教程和gist仍在准备。

Slide 11

With the release of Material Design, Toast can now have button and his brother - Snackbar is officially documented.

Slide 11/1

Easiest way to make Snackbar is to create class which will add or remove Snackbar's view to activity decor view over all other components. Just as Material Dialog it has several configurable settings, so please consider using Builder pattern.

Slide 11/2

Remarks:

* To show Snackbar use activity decor view group.
* For buttons use AppCompat.Button.Flat style (see Flat button section of this presentation).
* For text style use TextAppearance.AppCompat.Body1 from appcompat-v7 library.

Slide 11/3

Tutorial and gist are still in progress :)

Slide 11

随着Material Design的发布，Toast现在也可以拥有Button了，并且它的兄弟-Snackbar也诞生了。

Slide 11/1

实现Snackbar的最简单的方法就是创建一个类，它将在Activity中增加或移除Snackbar view。就像Material Dialog一样，他有几个设置项，所以要考虑使用Builder模式。

Slide 11/2

注意:
	使用activity的decor view group来显示Snackbar
	对于Button，使用AppCompat.Button.Flat（参见Flat button一节）
	对于字体样式，使用AppCompat-v7库中的TextAppearance.AppCompat.Body1

Slide 11/3

教程和gist仍在准备。

Slide 12

Not all material components can fit in 100 lines of code, some require few days of work. If you lack of time - that’s the case when third-party libraries are handy.

Slide 12

并不是所有的材料组件都能用100行左右的代码来实现，有的可能需要几天的时间。如果你缺乏时间-这样的话，使用第三方的库也是很方便。

lide 13

Material Design date and time pickers are complex widgets, which fortunately are available in google open source library.

Slide 13

Material Design日期时间选择器是个比较复杂的组件，但幸运的是在Google的开源库中你可以直接使用的。

Slide 13/1

First you need to grab a library from the url above and add it as a library module. API is the same as in native date and time pickers. 	

Slide 13/1

首先，你需要通过上面的url获取一个库，并将它设置为依赖库。API与原生的时间日期选择器的相同。

Slide 14

Chips View is only available as a third party library based on Google's internal chips library.

Slide 14

Chips View仅仅能作为一个基于Google的内置chips库的第三方库来用。

Slide 15

In Material Design Spec. progress now has several different states. Determinate and Indeterminate views are available as a third party library.

Slide 15

在Material Design规范中，progress现在也有了好几种不同的状态。Determinate and Indeterminate views也可以作为第三方库来使用。

Slide 16

Material Sliders now have number indicator, available as a third party library, without possibility to modify indicator shape.

Slide 16

Material Sliders现在也有了数字指示器了，也可以作为第三方库使用，但不能改变指示器的形状。

Slide 17

There are a lot of material tabs libraries, but all have some issues, like wrong indicator animation, ripple effect instead of reveal etc. I highly recommend to create your own tabs indicator :)

Slide 17

也有一些Material的tabs库，但是都有些问题，像错误的指示器动画，ripple效果而不是reveal等等。我高度推荐自己做一个tabs指示器。

Slide 18

Summary

* Use CardView or ShadowLayout if you need shadow for pre Lollipop.
* Almost all third party libraries have issues and only partly imitates Material Design spec behavior.
* Some components are easy to implement - it is worth it to make your own material-support library and reuse it.

Slide 18

总结：
	你若在Lollipop之前需要阴影效果，就使用CardView或者ShadowLayout。
	几乎所有的第三方库都存在问题，并且只是部分模仿了Material Design规范。
	一些组件很容易实现-制作你自己的Material-support库并使用它是很有值得的。



