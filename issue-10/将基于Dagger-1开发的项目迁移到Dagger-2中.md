将基于Dagger-1开发的项目迁移到Dagger-2中
---

> * 原文链接 : [Dagger 1 to 2 migration process](http://frogermcs.github.io/dagger-1-to-2-migration/)
* 原文作者 : [Miroslaw Stanek](https://about.me/froger_mcs)
* [译文出自 :  开发技术前线 www.devtf.cn](http://www.devtf.cn)
* 译者 : [chaossss](https://github.com/chaossss) 
* 校对者: [这里校对者的github用户名](github链接)  
* 状态 :  校对中

Dependency injection frameworks in Android - is there anyone who has never heard about it? At almost every Android dev conference someone talks about this software design pattern. I am a big fan of DI but there are also people who complain about it. The main reasons why they do this are:

我相信每一个 Android 开发者都听说过依赖注入框架，事实上几乎所有 Android 官方会议都讨论过软件设计模式。虽然我是依赖注入的脑残粉，但我不得不承认也有人不喜欢依赖注入，主要原因如下：

- **dependency injection frameworks are slow** - well, it was completely true a couple years ago in times of [RoboGuice](https://github.com/roboguice/roboguice), when the whole dependency graph were created and validated in runtime. Now, when we have [Dagger](https://github.com/square/dagger) it’s not (completely) true. In Dagger 1 much of work (graph validation) is done in compilation time and objects creation process is done without reflection (it’s worth mentioning that recently presented Roboguice 3 also does much of his work in compile-time). Yes, it’s still a bit slower than hand-written code, but in average Android app it is almost imperceptible.

- **依赖注入框架很慢** - 好吧，在使用 [RoboGuice](https://github.com/roboguice/roboguice) 的时代里，整个依赖图表会在运行时被创建和验证，使依赖注入框架确实会存在这样的问题。但现在，[Dagger](https://github.com/square/dagger) 让这一切发生了改变。在 Dagger 1 框架中，大量的工作（图表验证）在编译时被完成，而且对象创建也不需要通过反射机制完成（值得一提的是：最近发布的 Roboguice 3 也在编译时完成了大量的工作）。虽然 Dagger 1 框架的效率还是不如工程师亲自写的代码，但在大部分 App 里这都是可以接受的。

- **DI frameworks requires a lot of boilerplate** - it is true and it isn’t. Yes, we have to create additional code for injections, classes which provides dependencies etc. but thanks to them we don’t have to deal with objects constructors every time when we need them. Yes, in small projects DI frameworks are overkill, but benefits of dependency injection increase when you start dealing with scale.

- **依赖注入框架需要大量的模板** - 可以说它是对的，也可以说不是。确实，我们需要为提供依赖的类和注入添加额外的代码，但正因为我们完成了这样的工作，使我们不必在每次需要使用它们的时候通过构造器处理它们。我不否认依赖注入框架会在影响小型 App 的性能，但随着 App 内部结构变得复杂，依赖注入的好处会越来越明显。

- Other stuff like poor traceability, hard to read generated code etc.

- 其他缺点就是较差的溯源性，已生成代码的可读性差等等……

As I said, I’m a big fan of DI and these drawbacks don’t discourage me to use of this desing patterns in projects which I work on. Until recently I used Dagger 1 with no problems. But when we decided to rewrite the [Azimo](https://azimo.com/) (my newest project) completely and build it on top of DI framework, some disadvantages of Dagger 1 came to light. What exactly? We’ll be back to them shortly.

但我已经说了：我是依赖注入框架的脑残粉，刚刚提到的种种缺点并不会让我放弃在项目中使用它。直到最近我使用 Dagger 1 都没产生什么问题，但当我们决定完全重写 [Azimo](https://azimo.com/) （我的新项目），并将它运行在依赖注入框架时，Dagger 1 中存在的一些问题渐渐显现。这些问题到底是什么呢？且听我娓娓道来。

Fortunately for us Dagger 2 came to Not quite released, but stable, feature-completed state.

不过好消息是，Dagger 2 虽然还没有发布正式版，但 Dagger 2 已经有一个稳定，特征完备的版本了。

## Dagger 2

I won’t deep to much into technical details of the second major version this dependency injector. Instead, just check official [Dagger 2 website](http://google.github.io/dagger/) and Devoxx talk about [The Future of Dependency Injection with Dagger 2](https://www.parleys.com/talk/5471cdd1e4b065ebcfa1d557/) from Jake Wharton.

说实话，我并没有太深入地了解 Dagger 2 的实现细节，我只是稍微看了看官网的一些说明 [Dagger 2 website](http://google.github.io/dagger/) 和 Jake Wharton 的随笔-[The Future of Dependency Injection with Dagger 2](https://www.parleys.com/talk/5471cdd1e4b065ebcfa1d557/)。

What was a super-important for me, Dagger 2 almost automatically helps to solve a couple of problems which could take a lot of time with Dagger 1:

毕竟对我来说最重要的是，Dagger 2 几乎自动地帮我解决了 Dagger 1 中大量的长耗时任务。

## Project proguarding
## 对项目使用 Proguard

Yes, Azimo app has reached dex’s 64k methods limit. At the beginning we started to use [MultiDex solution](https://developer.android.com/tools/building/multidex.html) but according to its flaws we had to consider using Proguard. What forced us to this decision?

我不得不承认，Azimo 应用的方法数已经超过了 65536 个了（[Android Dex's 64k 限制](http://ingramchen.io/blog/2014/09/prevention-of-android-dex-64k-method-size-limit.html)）。刚开始开发 Azimo 的时候我们使用的是 [MultiDex solution](https://developer.android.com/tools/building/multidex.html)，但随着应用的迭代我们发现 MultiDex solution 存在一些缺陷，因此我们必须考虑使用 Proguard。为什么我们必须要作出这样的决定呢？

> 译者注：ProGuard是一个压缩、优化和混淆Java字节码文件的免费的工具，它可以删除无用的类、字段、方法和属性。可以删除没用的注释，最大限度地优化字节码文件。它还可以使用简短的无意义的名称来重命名已经存在的类、字段、方法和属性。常常用于Android开发用于混淆最终的项目，增加项目被反编译的难度。

Only the MultiDex.install(this); called from MultiDexApplication’s onCreate() method takes almost 4000ms on Nexus 7 with older Android version (4.4) (Lollipop supports MultiDex natively so it takes only 1ms on the same device). Also app build time increased dramatically (almost 2mins per gradle assembleDebug, even if we changed only single line of Java code). Why it takes so long? In short - MultiDex plugin has to scan sources everytime we change someting to decide which code should be put in first .dex file and which can be moved to another.

仅有在 MultiDexApplication 类的 onCreate() 方法中调用的 MultiDex.install(this) 在使用 Android 4.4 系统的 Nexus 7 设备中花费大约 4000ms 的时间（Lollipop 对 MultDex 的支持是基于内核的，所以在相同的设备中只需要1ms）。此外，应用的构建时间戏剧性地不降反增（即使我们只修改了一行代码，每一个 Gradel assembleDebug 仍需要2分钟左右的时间）。为什么耗时这么长？简单来说：每一次我们修改代码决定将哪些代码放到第一个 .dex 文件中以及哪些代码能够移动到其他 .dex 文件时，MultiDex 插件都需要扫描资源文件。

So we decided to use Proguard and boom… there is no simple receipe to handle Dagger 1’s generated code in proguard rules. Yes, we can use @Keep annotation from [Squad leader](https://bitbucket.org/littlerobots/squadleader) but still we had to spend some time to update our code and remember this rule in the future.

所以我们决定使用 Proguard，然后就爆炸了……因为在 Proguard 规范下没有用来处理 Dagger 自动生成代码的简单办法。所以我们得使用 [Squad leader](https://bitbucket.org/littlerobots/squadleader) 的 @Keep 注解来简化这个问题，但我们还是需要花费一定的时间更新代码，并将这条规范在注释中标注。

Dagger 2 answer: there is no single rule (!) required by Dagger 2 in proguard. Everything just works. Dagger 2 generates fully traceable code and doesn’t use reflection - it’s 100% Proguard friendly.

Dagger 2 回应：Proguard 中不存在 Dagger 2 所需要的单一原则。一切都是自然而然发生的。因为 Dagger 2 产生完全可溯源的代码，而且不需要使用反射 - 这对 Proguard 的友好度简直爆表。

## Other things
## 其他事项

Here are less (but still) important things which convinced us to Dagger 1 -> Dagger 2 migration:

现在还有一些不是非常重要（但又有用）的事项让我们确信将基于 Dagger 1 框架开发的代码转移到 Dagger 2 中：

- Code generated by Dagger 1 is hard to understand. Yes, we can trust the authors but sometimes it’s good to know what resides under the hood. Also this code isn’t fully traceable what means that we can’t use i.e. “find usages” functionality in our IDE.

- Dagger 1 自动生成的代码非常难度，虽然 Dagger 1 他爹值得信赖，但了解 Dagger 1 在底层到底留下了什么对我们也有好处。由于这些代码并不是完全可溯源的，那就意味着我们不能用 IDE 中类似 “find usages” 的功能。

Dagger 2 generates entire stack that looks as close to hand-written DI code as possible. Finally we can investigate this code to better understand how it works!

Dagger 2 生成了和程序员亲手实现的依赖注入代码几乎一样的整个栈。使代码具有完全可溯源性，便于我们更好地理解代码的运行机制。

- Maybe Dagger 2 is less flexible but the API is more clear and simple than Dagger 1. Our team still grows and in the moment when our app has been rewritten from scratch it’s important to understand whole architecture to move as fast as possible with as less bugs as possible. Thanks to Dagger 2 learning curve is a little less steep.

- 或许 Dagger 2 的拓展性变差了些许，但 Dagger 2 的 API 比 Dagger 1 清晰易用得多。我们的团队仍然在发展，在我们重写应用的时候，理解整个应用的架构过程必须尽可能快， Bug 要尽可能地少是很重要的。幸亏 Dagger 2 的学习曲线并不陡峭，这为我们节省了学习成本。

- Dependencies graph composition time. Maybe it isn’t big deal for us right now - graph in our app is composed in ~80ms on Nexus 7 (Android 4.4) device. But Dagger 2 reduced this time to ~40ms.

- 依赖图的构建时间。或许现在它对我们来说不是什么大问题 - 因为在 Nexus 7（Android 4.4 系统）构造应用的依赖图大概需要 80ms 的时间。但使用了 Dagger 2，只需要 40ms 依赖图就构建完成了。

# Dagger 1 to Dagger 2 migration process
# 将基于 Dagger 1 开发的项目迁移到 Dagger 2 中

A couple months ago Antonio Leiva created short series of posts ([post 1](http://antonioleiva.com/dependency-injection-android-dagger-part-1/), [post 2](http://antonioleiva.com/dependency-injection-android-dagger-part-2/), [post 3](http://antonioleiva.com/dependency-injection-android-dagger-part-3/)) which explains usage of dependency injection in Android project built with MVP (Model-View-Presenter) pattern. I decided to clone [his project](https://github.com/antoniolg/DaggerExample/) from Github and update it to Dagger 2.

Antonio Leiva 曾在几个月前写了有关如何在 Android 基于 MVP 模式开发的项目中使用依赖注入的系列博文([post 1](http://antonioleiva.com/dependency-injection-android-dagger-part-1/), [post 2](http://antonioleiva.com/dependency-injection-android-dagger-part-2/), [post 3](http://antonioleiva.com/dependency-injection-android-dagger-part-3/))。我觉得这个项目挺好的，于是将它下了下来，并让它的依赖注入框架更新为 Dagger 2。

## Dependencies graph
## 依赖图

To better understand how Dagger 1 works in this example I created image of dependencies graph used in DaggerExample project:

为了更好地理解 Dagger 1 是如何在范例中运行的，我用 DaggerExample 项目创建了依赖图：

![](http://frogermcs.github.io/images/12/dagger1-graph.jpg)

Now take a look at the same project, but with Dagger 2:
现在我们来看看在相同的项目中使用 Dagger 2 创建的依赖图：

![](http://frogermcs.github.io/images/12/dagger2-graph.jpg)

Can you see the similarities?

你能看到其中的相似之处吗？

The main worth mentioning difference between Dagger 1 and 2 are Compontents. In short they enumerate all of the types that can be requested by callers. But Component interfaces declare only that something is provided and modules declare how it is provided, so Modules are still responsible for creating objects. Components are just a public API for our graph.

Dagger 1 和 Dagger 2 两者的相似点中最值得一提的就是它们的组件。简单来说，它们把调用者可能请求的所有类型都枚举出来了。但组件接口只声明它为调用者提供了某些东西，以及这些东西由 Module 提供，所以 Module 仍然负责创建对象。组件只是依赖图的公有 API。

# Migration process
# 迁移过程

## Build, dependencies
## 创建依赖

First of all we have to update build.gradle files to add new dependencies. At the moment when I’m writing this post Dagger 2 is in active development and only Snapshot version is available. That’s why we have to add Sonatype snapshot repository:

首先，为了添加新的依赖，我们要更新 build.gradle 文件。当我在写这篇博文的时候，只有一个快照版本是可用的。这也是我们必须添加 Sonatype snapshot 库的原因：

**build.gradle:**

```java
	buildscript {
	    repositories {
	        mavenCentral()
	        maven { url "https://oss.sonatype.org/content/repositories/snapshots/" }
	    }
	    dependencies {
	        classpath 'com.android.tools.build:gradle:1.1.3'
	        classpath 'com.neenbedankt.gradle.plugins:android-apt:1.4'
	    }
	}
	 
	allprojects {
	    repositories {
	        mavenCentral()
	        maven { url "https://oss.sonatype.org/content/repositories/snapshots/" }
	    }
	}
```

The android-apt plugin assists in working with annotation processors and allow to configure a compile time only annotation processor as a dependency, not including the artifact in the final APK. Also it generates source paths for generated code which is visible and traceable by Android Studio.

Android-ADT 插件与注解处理器协作，并仅在注解处理器为依赖时允许配置编译时间而不把 artifact 添加到最后的 APK 中。当然了，它还为自动生成的代码生成了相应的原路径，在 Android Studio 中它们都是可见的并且可溯源的。

app/build.gradle file with dagger and android-apt plugin should look like below:

Dagger 的 app/build.gradle 文件和 Android-ADT 中的信息应该大致如下：

```java
	apply plugin: 'com.android.application'
	apply plugin: 'com.neenbedankt.android-apt'
	 
	android {
	    compileSdkVersion 22
	    buildToolsVersion "22.0.1"
	 
	    defaultConfig {
	        minSdkVersion 14
	        targetSdkVersion 22
	        versionCode 1
	        versionName "1.0"
	    }
	 
	    compileOptions {
	        sourceCompatibility JavaVersion.VERSION_1_7
	        targetCompatibility JavaVersion.VERSION_1_7
	    }
	    buildTypes {
	        release {
	            minifyEnabled false
	            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.txt'
	        }
	    }
	}
	 
	dependencies {
	    compile fileTree(dir: 'libs', include: ['*.jar'])
	    compile 'com.google.dagger:dagger:2.0-SNAPSHOT'
	    apt 'com.google.dagger:dagger-compiler:2.0-SNAPSHOT'
	    provided 'org.glassfish:javax.annotation:10.0-b28'
	}
```

## Modules

This is the simplest step in migration process. Modules become as simple as possible. They just need a @Module annotation (without any additional params), and @Provides annotation for providing objects.

接下来这一步可以说是迁移过程中最简单的一步了。Module 将尽可能变得简单，它们只需要为将要提供的对象添加 @Module（不需要任何额外的参数） 和 @Provides 注解。

Here is AppModule class:

这是 AppModule 类：

**Dagger 1:**

```java
	@Module(
	        injects = {
	                App.class
	        },
	        includes = {
	                DomainModule.class,
	                InteractorsModule.class
	        }
	)
	public class AppModule {
	 
	    private App app;
	 
	    public AppModule(App app) {
	        this.app = app;
	    }
	 
	    @Provides public Application provideApplication() {
	        return app;
	    }
	}
```

**Dagger 2:**

```java
	@Module
	public class AppModule {
	 
	    private App app;
	 
	    public AppModule(App app) {
	        this.app = app;
	    }
	 
	    @Provides public Application provideApplication() {
	        return app;
	    }
	}
```

What is the difference? injects param is moved to the Component, as same as includes. All Module params should be deprecated and removed soon.

两者的区别在哪里？区别在于：注入参数被移动到组件内，就像使用 include。所有 Module 参数都该尽快过时/移除。

##Components
##组件

As I said, it’s something new in Dagger 2. In short it’s kind of public API for our graph. Take a look again at the seconde picture of our graph. Here you have the implementation for each @Componentclass:

就像我刚刚说的，Dagger 2 中还添加了一些新的特性。简单来说把，就是依赖图的一些公有 API。大家不妨再回去看看刚刚 Dagger 2 的依赖图，这是每一个 @Componentclass 的实现：

**AppComponent:**

```java
	@Singleton
	@Component(
	        modules = {
	                AppModule.class,
	                DomainModule.class,
	                InteractorsModule.class
	        }
	)
	public interface AppComponent {
	    void inject(App app);
	 
	    AnalyticsManager getAnalyticsManager();
	    LoginInteractor getLoginInteractor();
	    FindItemsInteractor getFindItemsInteractor();
	}
```

**LoginComponent:**

```java
	@ActivityScope
	@Component(
	        dependencies = AppComponent.class,
	        modules = LoginModule.class
	)
	public interface LoginComponent {
	    void inject(LoginActivity activity);
	 
	    LoginPresenter getLoginPresenter();
	}
```

**MainComponent:**

```java
	@ActivityScope
	@Component(
	        dependencies = AppComponent.class,
	        modules = MainModule.class
	)
	public interface MainComponent {
	    void inject(MainActivity activity);
	 
	    MainPresenter getLoginPresenter();
	}
```

We used @ActivityScope annotation. In short it’s replacement for @Singleton annotation used in local subgraphs. In Dagger 1 for example we had singleton object of LoginPresenter. And it was true, that this object was a singleton. But it was some kind of local singleton - it lives as long as scoped graph (which was set to null in onDestroy() method in example code).

大家可以看到我使用了 @AcivityScope 注解。简单来说，它是本地子图中 @Singleton 注解的代替品。以 Dagger 1 为例吧，LoginPresenter 类中我们有一个单例对象，虽然对象却是是一个单例，但某种程度上它总像一个本地单例 - 因为它只存活于图的作用域中（示例代码中，该单例会在 onDestory() 方法中被设置为 null)。

@ActivityScope is used just for semantic clarity and it’s defined in our code:

@ActivtiyScope 只用于语境清晰的环境中，它在代码中的定义如下：

```java
	@Scope
	@Retention(RetentionPolicy.RUNTIME)
	public @interface ActivityScope {
	}
```

## ObjectGraph
## 对象图

There is no more ObjectGraph in Dagger 2. Now Components took his place. Exactly the code generated from them with DaggerComponent_ prefixes. It means that now we have to deal with generated code (but only in this place).

Dagger 2 中不再有对象图，因为对象图被组件取代了。确切来说，Dagger 2 框架将自动生成以 DaggerComponent_ 为前缀的代码。这就意味着我们现在必须处理生成代码（但只在这里处理）。

Keep in mind that DaggerComponent_ classes are generated only when whole code is valid, so you won’t see them until you fix all errors.

我们必须记住：DaggerComponent_ 类只有在所有代码有效的情况下被生成，所以在你解决所有 Error 之前你不会看到它。

How it looks in practice?

那它具体长什么样呢？

```java
	public class App extends Application {
	 
	    private AppComponent component;
	 
	    @Inject
	    AnalyticsManager analyticsManager;
	 
	    @Override
	    public void onCreate() {
	        super.onCreate();
	        setupGraph();
	        analyticsManager.registerAppEnter();
	    }
	 
	    private void setupGraph() {
	        component = Dagger_AppComponent.builder()
	                .appModule(new AppModule(this))
	                .build();
	        component.inject(this);
	    }
	 
	    public AppComponent component() {
	        return component;
	    }
	 
	    public static App get(Context context) {
	        return (App) context.getApplicationContext();
	    }
	}
```

In lines 16-18 our graph is built. It replaces ObjectGraph.create(getModules()); from Dagger 1.

我们的图将通过16-18行代码生成，取代了 Dagger 1 的 ObjectGraph.create(getModules()) 代码。

Line 19 injects App object into the graph (and all @Inject in this class are satisfied in that moment).

第19行代码将 App 对象注入到途中（而且此时类中所有 @Inject 注解都是可信的）

Here is an example of local graph (from MainActivity.class):

这是一个本地图（MainActivity）的范例：

```java
	protected void setupComponent(AppComponent appComponent) {
	    Dagger_MainComponent.builder()
	            .appComponent(appComponent)
	            .mainModule(new MainModule(this))
	            .build()
	            .inject(this);
	}
```

MainComponent depends on AppComponent so we have to provide this object explicitly. If Module has no default constructor you have to provide them too (like MainModule).

MainComponent 依赖于 AppComponent，使我们必须准确地提供这个对象。如果 Module 没有默认的构造器，你必须提供（就像 MainModule)。

And the migration is done. In case we missed something here is complete [pull request](https://github.com/antoniolg/DaggerExample/pull/5) with magration process. Keep in mind that this article doesn’t cover more complex solutions and all capabilities of Dagger 2. Here you have some links which can help you to better understand how Dagger 2 and dependency injection works:

那么迁移工作到这里就算完成了。提交完整的迁移过程的 [pull request](https://github.com/antoniolg/DaggerExample/pull/5) 能防止我们遗漏某些东西。大家必须记住：这篇文章没有讲解更多复杂的解决办法，以及 Dagger 2 的所有特性和功能。下面是一些能帮你更好地理解 Dagger 2 和依赖注入框架运行机制的链接：

[The Future of Dependency Injection with Dagger 2](https://www.parleys.com/talk/5471cdd1e4b065ebcfa1d557/)
[Dagger 2 doc by Gregory Kick](https://docs.google.com/document/d/1fwg-NsMKYtYxeEWe82rISIHjNrtdqonfiHgp8-PQ7m8/edit)
[Dagger 2 official website](http://google.github.io/dagger/)

And that’s all. Thank you for reading. I hope to deep Dagger 2 even more, so see you soon! 😃

那这篇博文就到此为止啦，感谢你能耐心看完这篇文章，我希望能更深入地挖掘 Dagger 2 的技术细节，我相信大家很快会看到我的新博文的！😃

## Source code
## 源码

Full source code of described project is available on Github [repository](https://github.com/frogermcs/DaggerExample).

[Github](https://github.com/frogermcs/DaggerExample) 上面有整个项目的源码。