在使用Android Studio的过程中，你可能不知道的10件事
---

* 原文链接 : [(About) 10 Things You (Probably) Didn’t Know You Could do in Android Studio](https://medium.com/google-developers/about-10-things-you-probably-didn-t-know-you-could-do-in-android-studio-de231071b375#.im9teya8w)
* 原文作者 : [作者](https://medium.com/@retomeier)
* 译文出自 : [开发技术前线 www.devtf.cn](http://www.devtf.cn)
* 转载声明: 本译文已授权[开发者头条](http://toutiao.io/download)享有独家转载权，未经允许，不得转载!
* 译者 : [captainJeck](https://github.com/captainJeck) 
* 校对者: [这里校对者的github用户名](github链接)  
* 状态 :  校对中


We all have better things to do with our time than count the *exact*
number of Android Studio pro-tips in a 3 minute video — check it out for
yourself and see how many are new to you.

我们有更重要的事去做而不是在这3分钟的视频里数有多少的Android Studio开发技巧 － 看看有多少是你未知的。

> The video moves pretty quickly, so keep reading to see most of the
> tips and tricks featured in written / animated GIF form for easy
> reference.

> 视频可能播放的比较快，所以更多的使用技巧可通过阅读文字或GIF动画来了解。

An over-dependence on using your mouse while coding can have results
more serious than just lower productivity. These pro tips are designed
to help you write less code, and make every keystroke count, so you can
avoid situations like this one.

在编码时，过度的依赖鼠标不仅仅会降低生产效率。这些技巧本来就是为了帮助你写更少的代码，所以，你完全可以避免以下这种情况。

![Thanks Obama](https://cdn-images-1.medium.com/max/800/1*-wEOUYr835kBIb3oT0Syyg.gif)

![感谢 Obama](https://cdn-images-1.medium.com/max/800/1*zmhPiKsZCVAG-OBUrc5z9A.gif)

Most of these tips are available as part of IntelliJ, on which Android
Studio is built. The most important shortcut to remember in Android
Studio is CMD-SHIFT-A (or CTRL-SHIFT-A if you’re on a Windows or Linux
PC).

Android Studio是基于IntelliJ构建的，所以IntelliJ中的很多技巧都是可以使用的。在Android Studio中有一个非常重要的快捷键需要你记住，它就是CMD+SHIFT+A(如果你使用的是Windows或Linux的话，快捷键为CTRL+SHIFT+A)(译注：其实为find action快捷键)

Use CMD-SHIFT-A or CTRL-SHIFT-A to find actions or options
After hitting that shortcut, you can just typing keywords and available
actions and options will be available for your each selection. It’s a
great way to start using new features before you’ve memorize their
shortcuts.

当你忘记那些快捷键的时候，你可以使用CMD+SHIFT+A或CTRL+SHIFT+A来查找相应的操作，你可以简单的输入关键字来匹配操作或可选项。在你没有记住这些快捷键之前，这是使用这些新特性的最好方法。

You can use a similar approach anywhere there’s a long list of options.
If you’re trying to find a file in the project hierarchy, or select an
option from large menu such as *Refactor This…*, just start typing and
it’ll start searching and filtering results.

你可以在任何地方使用类似的方法，它会提供你很多的可选项。如果你想在项目目录中查找一个文件，或在长长的菜单列表中查找一个类似*Refactor This…*的操作，只要简单的输入就可以实时的查找并过滤结果。

#### Using Tab to *Replace* Existing Methods and Values with Autocomplete Selections

#### 通过Autocomplete Selections使用Tab键替换已有方法或属性

![Pressing Tab replaces existing methods and values rather than just
inserting a new one](https://cdn-images-1.medium.com/max/600/1*AhdlQUqM71fvvGG_v8b6dQ.gif)

![使用Tab键替换现有方法或值而不是插入一个新的](https://cdn-images-1.medium.com/max/600/1*AhdlQUqM71fvvGG_v8b6dQ.gif)

Bringing up autocomplete with CTRL-SPACE (or CTRL-SHIFT-SPACE for
options of the expected type) is probably one of the most commonly used
shortcuts in Android Studio.

使用CTRL-SPACE快捷键唤醒`autocomplete`是Android Studio中最常用的快捷键之一。

And everyone’s experienced the mild annoyance of using this technique to
choose a new method, or select a different variable, hitting enter, and
having the new selection get inserted in front of the existing one.

如果我们在选择一个新方法或不同变量的时候用enter键，那么有一个不友好的体验，就是新的选项是插入在旧方法之前的。

If you hit TAB instead of ENTER, it’ll *replace* the existing method or
value instead. You’re welcome.

如果你使用tab键而不是enter键，那么会**替换**现有方法或值。不用谢。

#### Two Text Selection Tricks

#### 2个文本选择技巧

Up, down, left, right, and a combination of CTRL, SHIFT, and Fn covers
most of your cursor navigation and selection needs — but the ALT
modifier adds some new and unexpected flavor.

ctrl、shift和fn键组合上、下、左、右覆盖了大多数的导航和选择的快捷键 － 而alt键加入了一些意想不到的新特性。

You can use ALT-UP and ALT-DOWN to extend and contract your selection by
“node” — effectively letting you modify selection by scope.

你可以使用ALT+UP和ALT+DOWN键来扩大或缩小你选择的节点范围 － 让你更高效的修改选择的范围。

Meanwhile, ALT-SHIFT-UP and ALT-SHIFT-DOWN will let you move a selection
up and down respectively, conveniently moving other code out of the way
without the need to resort to anything as pedestrian as cut and paste.

ALT+SHIFT+UP 和ALT+SHIFT+DOWN键可以向上、向下移动你选择的代码，像剪切粘贴一样方便，而不需要其他任何手段。

#### Postfix Code Completion and Live Templates

#### Postfix Code Completion 和 Live Templates 

If I’d been paid a dollar for every for loop, if-statement, and log
statement I’ve typed I’d… actually that might work out to be pretty
close to accurate.

如果你需要写一个for循环、if表达式或log日志，你以前会...实际上，我们有更好地解决方案。

In the spirit of getting paid more for typing less, take advantage of
postfix code completion and Live Templates to insert some of the most
common code patterns with quick shortcuts.

本着少输入的原则，使用postfix code completion 和 Live Templates的快捷方式插入一些通用代码模版

Use postfix code completion lets you transform an already typed
expression into another one.

使用 postfix code completion让你把已经输入的表达式转换成一个全新的。

![](https://cdn-images-1.medium.com/max/800/1*Mt2-SylWiSTRZ3kRece21w.gif)

You can create a for-loop over a list using the *.fori* shortcut, or a
boolean variable (or expression) into an if statement using *.if*
(or *.else*). You can use see all the valid postfixes available for a
given context by typing CMD-J (or CTRL-J on Windows / Linux).

你可以使用`fori`创建一个for循环，或用`.if`（`.else`）把boolean值转换成if表达式.你可以使用CMD＋J（windows、linux平台使用CTRL＋J）查看所有的可用后缀值。

![](https://cdn-images-1.medium.com/max/600/1*JkrYXGs1AxZAbK0sCLrJAQ.gif)


For more complex patterns, Live Templates let you use shortcuts that are
available as autocompletion options that will insert templatized
snippets into your code. For example, using the Toast shortcut makes it
easy to add a new toast, where you only need to specify the text to
display.

对于更高级的模式，Live Templates让你使用快捷方式生成可插入你自己代码的选项。例如，使用`Toast`可以轻松的创建一个toast提示，你只需要编写需要展示的文本。

There are dozens of generic and Android specific Live Templates,
including a selection of logging shortcuts — or you can [create your
own](https://medium.com/google-developers/writing-more-code-by-writing-less-code-with-android-studio-live-templates-244f648d17c7#.2yzgzmdke) to simplify best-practice patterns or boilerplate
within your own code.

这里有几十个通用的和Android特有的Live Templates，包括日志选择快捷键 - 或者你自己在代码中创建简单实用的模板。

#### Custom Rendering for Objects when Evaluating Expressions 
#### 评估表达式时自定义渲染对象

When debugging code at runtime — looking at variable values at a
breakpoint or evaluating expressions — objects are displayed using
their .toString() values. If your variable is a String or a primitive
type that works well, but for most objects it’s not particularly
helpful.

当你在debug模式时－你需要在断点处查看属性值或估算表达式－对象的话展示的是`.toString`信息。如果你的变量是一个String类型或一个原始类型那没有问题，但对大多数对象是没有用的。

This is especially true of collections of objects, which are typically
displayed as a list of “ClassName:HashValue”. To understand the state,
you need to dig into each object.

如果是一个对象集合的话，问题就很明显了，它会显示为`ClassName:HashValue`。想了解状态，必须深入每一个对象。

Instead, you can create a custom renderer for any object type.
当然，你可以为每个对象创建一个自定义的展示。

![](https://cdn-images-1.medium.com/max/800/1*D6xJW1DalD9K8miVemhmAg.gif)


Right click on the object, select “View as” → Create… and define the
expression you want to use to render that object when debugging.

在对象上右击，选择`View as`然后你可以创建并修改在debug时你想渲染对象的表达式。

#### Structural Search, Replace, and Inspection 
	
#### 结构搜索，替换和检验

Structural search and replace let you search for, and replace (respectively),
code patterns without resorting to Regular Expressions.

结构体搜索和替换无需正则表达式的代码模式。

![Structural Replace Inspections Let You Create Your Own Lint Checks with Quickfixes](https://cdn-images-1.medium.com/max/600/1*ZQj0QivAp_SQaOfaeDp4Kg.gif)

![Structural Replace Inspections 可以创建你自己的Lint检查](https://cdn-images-1.medium.com/max/600/1*ZQj0QivAp_SQaOfaeDp4Kg.gif)

Even more usefully, you can enable Structural Search Inspection. You can
then save a Structural Search Templates that will flag code that matches
that pattern warning, displaying the hint text you provide.

更有用的事，你可以使用Structural Search Inspection。你可以保存一个Structural Search Templates来检测你的代码，并显示你提供的提示。

Use it to flag anti-patterns within your code (or code you’re
reviewing).

用它标记出你的代码（或你正在审查的代码）中不符合模板的地方，

Even more powerfully, you can create Structural Replace Templates. Like
Structure Search Templates, the pattern will be flagged as a
warning — but in this case, the replacement code will be offered as a
quick fix!

更有劲的是，你可以创建Structural Replace Templates。像Structure Search Templates一样，它会跟你一个警告 - 不过这种情况，你必须快速处理。

This is perfect for creating quick fixes for deprecated code, or common
anti-patterns in code your reviewing, or which is being submitted by
other team members.

创建一个快速修复对于过期的、代码审查时的反模式、其他团队成员提交的代码来说真是太棒了。

------------------------------------------------------------------------


There are hundreds of tips and tricks to make your Android Studio
experience faster, more productive, and (mostly) mouse-free. Subscribe
to [Android Developers on
YouTube](https://www.youtube.com/user/androiddevelopers), and tune in to [Android Tool
Time](https://www.youtube.com/watch?v=xxx3Fn7EowU&list=PLWz5rJ2EKKc_w6fodMGrA1_tsI3pqPbqa) for more Android Tool Time Pro Tips.

[Android App
Development](https://medium.com/tag/android-app-development?source=post) [Android
Studio](https://medium.com/tag/android-studio?source=post) 
[Android](https://medium.com/tag/android?source=post)

这里有数以百计的提示和技巧让你拥有更快，更有效并且（大多数）不需要鼠标的Android Studio使用体验。请订阅[Android Developers on
YouTube](https://www.youtube.com/user/androiddevelopers),收听[Android Tool
Time](https://www.youtube.com/watch?v=xxx3Fn7EowU&list=PLWz5rJ2EKKc_w6fodMGrA1_tsI3pqPbqa)获取更多的Android工具高级使用提示。
[Android App
Development](https://medium.com/tag/android-app-development?source=post) [Android
Studio](https://medium.com/tag/android-studio?source=post) 
[Android](https://medium.com/tag/android?source=post)




