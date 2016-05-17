如何减小PNG图片大小
---

> * 原文链接 : [Reducing PNG file Size](https://medium.com/@duhroach/reducing-png-file-size-8473480d0476#.o9rn09rne)
* 原文作者 : [Colt McAnlis](https://medium.com/@duhroach)
* 译文出自 : [开发技术前线 www.devtf.cn](http://www.devtf.cn)
* 转载声明: 本译文已授权[开发者头条](http://toutiao.io/download)享有独家转载权，未经允许，不得转载!
* 译者 : [rogero0o](https://github.com/)
* 校对者: [这里校对者的github用户名](github链接)  
* 状态 :  未完成 / 校对中 / 完成

# Reducing PNG file Size

# 减小PNG图片大小

One of the benefits of my role here at Google is that Iget to troll through a lot of Android applications, and look for common places where people might be able to improve their performance.

在谷歌工作的一个优势就是我可以看到很多的 android 应用，寻找它们中能够被提高的地方。

Lately, I’ve been noticing the growth of a frightening trend : Bloated PNG files.

然后，我察觉到了一个可怕的趋势：日益膨胀的PNG图片。

As I talked about last time, PNG is a pretty awesome, flexible image file format. It’s got great quality control, and supports transparency. As such, it’s become the go-to for transparency-seeking developers for a few decades now.

上一次我提到他们时，PNG是一个非常棒的、异常灵活的图片格式。它能很好的控制质量，并且支持透明度。因此，它成为开发人员所寻求的支持透明度的图片格式已经数十年。

The problem is that it’s pretty easy to make bloated PNG files; heck, just adding two pixels to the width of your image could double its size. So, it’s easy to assume that most of the PNGs out there just haven’t been given the love & care they deserve.

最大的问题是我们很容易就将PNG图片变得异常大，只是将宽度增加两个像素就可能会将图片大小扩大一倍。所以，很容易认识到我们使用的所有PNG图片并没有得到应得的照顾。

So, now that I’ve trolled through about 100 APKs, I’ve decided to pass along my top suggestions on reducing PNG file size. These suggestions are based on things I’ve seen in real apps that real humans are using. YMMV if your app is only used by robots, or small squirrels.

所以，现在我已经调查过将近100个 apk ，我决定将我减小 png 图片的建议传递下去。这些建议是基于我在人们正在使用的 APP 中发现的 。 不过如果你的应用只是被机器人使用，或是十分小众的应用，那么这些建议将因人而异。

# You should be using an optimizer tool

Once you understand the PNG file format, it becomes clear that there’s some obvious areas of improvement that could result in smaller file sizes:

* Removing unneeded chunks
* Reducing unique colors
* Optimizing line-by-line filter choice
* Optimizing DEFLATE compression

一旦你理解PNG文件格式，你将会知道有一些很明显的方法是可以减小图片文件大小的：

* 移除不必要的区域块
* 减少不同的颜色
* 优化逐行过滤选项
* 优化默认压缩

And this isn’t new information. PNG optimization has been a common problem for a long time; 20 years ago, Ken Silverman wrote one of the first popular PNG optimizers, PNGOUT. (Which became the backbone of the famous Duke Nukem 3D engine.) Since then, there’s been a lot of new PNG optimizers that have hit the scene; A quick google search will bring up a plethora of options for you to choose from:

PNGQuant, ImageMagick , PNGGauntlet, PNGOut, PNGCrush, OptiPNG, CryoPNG, PNG Compressor, Yahoo Smush.it, PNGOptimizer, PunyPNG, TinyPNG, PNGWolf, Advpng, DeflOpt, Defluff, Huffmix, TruePNG, PNGnq-s9, Median Cut Posterizer, scriptpng, pngslim, zopfliPNG

这些并不是新的知识，PNG 优化问题许久以前已经是一个老大难问题，20年前， Ken Silverman 写了第一篇十分出名的 PNG 优化文章，[PNGOUT](http://advsys.net/ken/utils.htm) . (成为了著名的毁灭公爵3D引擎的基础) 后来，许多 PNG 优化方案层出不穷，用 google 搜一下你将会看到你将会有这么多的选项：

PNGQuant, ImageMagick , PNGGauntlet, PNGOut, PNGCrush, OptiPNG, CryoPNG, PNG Compressor, Yahoo Smush.it, PNGOptimizer, PunyPNG, TinyPNG, PNGWolf, Advpng, DeflOpt, Defluff, Huffmix, TruePNG, PNGnq-s9, Median Cut Posterizer, scriptpng, pngslim, zopfliPNG

The trick here, is that in the full spectrum of things a tool could do, each one of these tools does a subset of it; So there’s no “best tool” for the job, so make sure you’re spending the time to evaluate which one works best for you, and then adopt the hell out of it.

这其中的门道就是，似乎一个工具就能把所有的事情都给做了，每个工具都做了一系列的工作。所以不存在最好的工具来完成这项工作，请花时间谨慎选择评估最适合你的工具，然后采用他。

FWIW, A personal favorite of mine on that list has to be zopfliPNG. It reduces PNG file size by providing a more efficient, and powerful deflate stage in the compressor, allowing it to find better matches in your data. It can decrease the PNG file-size by 5%, without impacting image quality in any form… sure it’s significantly slower, but it’s impressive that improvements can still be made to the old-school DEFLATE codec.

无论如何，就我个人的喜好而言在这个列表中最好的将是 zopfiliPNG 。它通过提供更加高效、强大的压缩算法来减小 png 图片的大小，而这个算法是可以是从文件数据中匹配出来的。这将减小 PNG 图片大概5%的大小，并且完全不会影响图片的质量。虽然它的压缩速度的确比较慢，但是它强大的改进效果还是令人印象深刻的。

The gist here, is that if you’ve got a lot of data coming through your application, you should have a PNG optimization tool in your upload / distribute pipeline, if only to keep the crazy at bay.

这里的重点是，如果你应用有非常大的数据需要显示，你应该在上传、分发这些图片之前就使用 PNG 优化工具进行优化压缩，来防止 PNG 图片的日益膨胀。

# Reducing Colors

# 减少颜色

Now, if the above tools just aren’t working for you, or you’d like to adopt a more manual approach to improving some of your assets _before_ it gets tossed to one of the above tools, then it’s worth taking matters into your own hands.

现在，如果以上的工具都没什么效果，或者你想在使用这些工具之前做一些手动的优化，那么自己动手也是不错的选择。

Even though there’s a lot of things you could manually do, I suggest that you should only focus on reducing the number of unique colors in your image and then let a tool take it the rest of the way.

尽管有许多事情是可以手动做的，我仍然建议你应该专注在减小图片颜色种类上，然后使用一个工具来完成后面的工作。

The reason for your focus here, is that reducing unique colors directly influences the compression potential at every other stage of the pipeline; and tools can do the rest.

让你专注在减小图片颜色的原因是，这能直接影响在各个阶段的压缩潜力，让工具完成剩下的工作。

See, the filtering stage of the PNG compression step is powered by how variant adjacent pixel colors are to each other. As such reducing the number of unique colors in the will reduce variation in adjacent pixels, decreasing the dynamic range of values that are spit out from filtering.

由此可见，相邻像素的颜色值是影响压缩步骤中滤镜过程的主要因素。这样减少在相邻的象素的颜色值，将减少特有的颜色的数量，减小颜色筛选的动态范围。

As a result, the DEFLATE stage will find more duplicate values, and be able to compress better.

这样做的结果就是，其余默认的压缩步骤将发现更多相同的颜色值，使得压缩效果更好。

It’s worth noting though, that reducing the number of unique colors, we’re effectively applying a lossy encoding stage to our image. THIS is why you should handle this process manually. Tools have a difficult time understanding human perceptual quality, and in some cases, small errors to a tool can look like huge errors to a human eye. But, if done the right way, it shouldn’t be noticeable to the user, and can save a huge amount of space.

值得注意的是，虽然我们减少了图片中不同颜色的颜色值，但是这是一个对图片有损的步骤。这就是为什么需要你手动处理这部分工作的原因。压缩工具很难理解人们在各种情况下的实际需求，在某些情况下，一些工具中的小错误在人们眼里看起来将是十分巨大的问题。但是，如果处理得当，这将不被用户所察觉，而且能够减小巨大的空间。

# PSA: Choose the right pixel format

# PSA:请选择正确的像素格式

This should go without saying.. But I’ve seen examples in a few APKs that prove otherwise:

You should make sure to use the right pixel format for your PNG file.

这应该不用我提醒，但我在并非少数的APK中看到了错误的例子：

你应该确定你的PNG图片使用的像素格式是正确的。

For example, if you don’t have alpha in the image, then using the RGBA 32bpp option is a waste of an entire ¼ of your image; instead, use the 24bpp truecolor format (or just use JPG).Likewise, if your image just contains grayscale data, you should only be storing it as 8bpp.

例如，在图像中你不需要有透明度的值，那么使用 RGBA 32bpp 的格式选项将会浪费整整四分之一的大小，而如果使用 24bpp 的真彩（或者直接使用jpg），你将节省这些空间。同样的，如果你的照片是黑白的，你只需要使用 8bpp 的格式。

Basically, make sure you’re not unintentionally bloating your PNG file by using the wrong type of pixel format.

基本上，请确保你不会在无意中使用了错误的像素格式使你的PNG图片大小变得巨大。

# Indexed images, FTW!

# 使用索引图像！

Moving on, color reduction should always start with taking a stab at trying to optimize your colors so that it could be defined using the INDEXED format. INDEXED color mode, basically chooses the best 256 colors to use, and replaces all your pixels with an index into that color palette. The result, is a reduction from 16 million colors (24bpp) to 256, which is a significant savings.

继续，减少颜色值的第一步应该是优化颜色让图片能定位为索引图像的格式。索引颜色模式，主要是使用256种最常用的颜色值，并且将图片中的所有像素替换为这256种颜色的索引代码。其结果就是将一千六百万种颜色（24bpp）转换成256中，这将是一个明显的改进。

Here’s an example image, and it’s indexed variant:

![image](https://cdn-images-1.medium.com/max/800/1*AaAgW3WKzGcWUD0a8wbmQQ.png)


The example Google Doodle above was exported in Photoshop’s “save for web” feature, and the image format was set to PNG8, which created this color palette for the image:

![image](https://cdn-images-1.medium.com/max/800/1*ifzr3HwaURqE-3okFHRe9g.png)


Basically, by moving to indexed images, you’ve replaced the unique color at each pixel, with a pointer into the palette instead. The result is moving from a 32bit per pixel to 8 bits per pixel, which is a nice first-step file size reduction.

This mode creates further savings when you consider how the filtering and deflate stages are applied:

1. The number of unique pixels has been reduced, meaning that there’s a higher probability that adjacent colors will point to the same color
2. Since the number of similar adjacent colors increases, the filtering stage will produce more duplicate values, such that the LZ77 phase of DEFLATE can compress it better.

If you can represent your image as a paletted image, then you’ve gone a great way to significantly improving the file size, so it’s worth investigating if the majority of your images can be converted over.
