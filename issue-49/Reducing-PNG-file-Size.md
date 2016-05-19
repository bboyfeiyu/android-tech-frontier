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

下面是一个示例的图片，还有索引的数量：

![image](https://cdn-images-1.medium.com/max/800/1*AaAgW3WKzGcWUD0a8wbmQQ.png)

The example Google Doodle above was exported in Photoshop’s “save for web” feature, and the image format was set to PNG8, which created this color palette for the image:

这个 Google 的涂鸦图片是通过 Photoshop 的以网络格式保存图片功能保存的，它的图片格式是 PNG8 ，足以显示这个调色板。

![image](https://cdn-images-1.medium.com/max/800/1*ifzr3HwaURqE-3okFHRe9g.png)

Basically, by moving to indexed images, you’ve replaced the unique color at each pixel, with a pointer into the palette instead. The result is moving from a 32bit per pixel to 8 bits per pixel, which is a nice first-step file size reduction.

基本上，通过转变为索引图像，图片中的每一个像素的颜色值都将被替换成一个索引值。其结果就是将每一个像素的大小从 32bit 减小到 8bit ，作为减小图片大小的第一步成效还是不错的。

This mode creates further savings when you consider how the filtering and deflate stages are applied:

1. The number of unique pixels has been reduced, meaning that there’s a higher probability that adjacent colors will point to the same color
2. Since the number of similar adjacent colors increases, the filtering stage will produce more duplicate values, such that the LZ77 phase of DEFLATE can compress it better.

当你考虑如何实现过滤和压缩阶段时，这种模式将提供进一步的压缩效果：

1. 不同的像素颜色数量已经被减小，意味着更多的相邻的像素将有同样的颜色值。
2. 因为相似的相邻颜色数量的增加，过滤阶段将会产生更多相同的值，这样 LZ77 压缩阶段就可以得到更好的压缩效果。

If you can represent your image as a paletted image, then you’ve gone a great way to significantly improving the file size, so it’s worth investigating if the majority of your images can be converted over.

如果你的图片能被处理成上图调色板一样的格式，那么你已经显著减小了图片的文件大小，所以花时间查看你项目中的图片是否能被优化是十分值得的。

# Optimizing fully transparent pixels

# 优化完全透明的像素

One of the nice features of INDEXED mode, is that you can denote specific colors in your palette to act as “transparent”. When the PNG file is decoded into RGBA in main memory, transparent pixels will be set accordingly. What’s interesting here is that this transparency mode is entirely binary; a given pixel is either visible, or not.

索引模式的一个很棒的功能就是你可以将调色板中特定颜色转换为“透明”。当 PNG 图片在内存中被解码成 RGBA 图片时，相应的透明像素将被被设置。有趣的是,这种透明模式完全是二进制的;给定像素是可见的,或者不是。

![image](https://cdn-images-1.medium.com/max/800/1*Yq3LFOb4NmzMHed0AJ-4Aw.png)

An Indexed PNG file, with multiple identified transparency values

一张索引图片，有多个识别透明度值

This type of “punch through” transparency is pretty efficient in terms of compression; Normally, it’s used for large areas of the background that are transparent, and as such, there’s lots of self-similar pixels that the PNG Compressor can take advantage of.

这种 “punch through” 类型的透明度在压缩中是十分有效的，这一般使用在大片的透明背景中，例如上图中，就有许多在压缩过程中能被优化的相同的像素

But it’s only available in indexed mode. There are situations where you want to use the same “punch through” transparency, but need to use full RGB mode as well.

但是这只有在索引模式下才能被设置，在许多情况下你想要使用 “punch through” 透明度，但是同时也需要使用完整的 RGB 格式。

In these cases, it’s easy to make the mistake of not properly masking out your invisible pixels. Consider the example below; both images support transparency & Truecolor, but one of them is significantly smaller.

在这种情况中，很容易犯的一个错误是不适当的屏蔽不可见的像素。在以下的例子中，两张图片都支持 透明/真彩 ，但是其中一张明显更小。

![image](https://cdn-images-1.medium.com/max/800/1*CdJdTqpCKr7gC3zOkuILDA.png)

Two files that look the same, but have significantly different file sizes.

这两张图片看起来一样，但是大小确完全不同。

The reason for this size difference becomes apparent, when you disable the alpha channel:

当你禁用透明度通道时，两张图片的大小区别将变得非常明显。

![image](https://cdn-images-1.medium.com/max/800/1*ZpTATiM39cPjSgyVEaLTeA.png)

While these two images look the same when being displayed on screen, the left image has a full set of color data in the RGB channel for pixels which will eventually be Alpha’d out. Even though the user doesn’t see this information, the compressor still has to deal with it.

当这两张图片显示在屏幕上时，左边图片的像素在 RGB 通道都拥有完整的色彩数据以绘制出透明度。尽管用户不会看到这些数据，压缩中任然需要对这些数据进行处理。

Even though the Alpha channel will only allow some portion of the image to be rendered, there’s a FULL SET of pixel data in the RGB layer, meaning that the filtering & DEFLATE stages will still have to go through and compress all that data.

尽管透明通道将仅仅允许部分图片被呈现，完整的像素信息也会被存在 RGB 层中，这意味着过滤/默认阶段仍然将要对完整的数据进行优化处理。

Instead, if you know those pixels won’t be seen, make sure they are homogeneous. When we fill the non-visible pixels with a single value; So we’ve flattened the parts of the image that won’t be seen.

相对的，如果你知道这些像素将不被察觉，请确保他们的均匀的。这样我们可以用一个值来代替这些不可见的像素，通过这样来处理那些图像中不被看到的部分。

The result is that more of each row is a single color, and thus the interpolation predictor, and Deflate stages will produce better compression. Basically, this is a fun little hack to let you get punch-through transparency, in true-color mode, with a smaller file-size footprint.

这样做的结果就是每一行都是一个颜色，因此在默认的差值预测阶段中，将会产生更好的压缩效果。基本上这是一个在真彩模式中让你得到 “punch-through” 透明度的一个黑科技，并且将得到一个更小的图片文件。

# Lossy Pre-process

# 有损预处理

The indexed mode in PNG is fantastic, but sadly, not every image will be able to be accurately represented with only 256 colors. Some might need 257, 310, 512 colors, or 912 colors to look correct. Sadly, since Indexed mode only supports 256 colors, these images have to default all the way to 24bpp, even though only a subset of colors are actually needed.

索引模式的 PNG 图片的确是神奇的，但是可惜的是，并非所有的图片都能用 256 种颜色完全显示。有些也许需要 257 ，310, 512 种甚至 912 种颜色才能正确的显示出来。由于索引模式只支持256种颜色值，所以尽管也许只有一小部分颜色值是需要的，我们也只能用完整的 24bpp 格式。

Thankfully though, you can get pretty close to index savings by reducing colors manually.

但是尽管如此，手动的减少颜色值也能让你接近索引图像的保存方法。

The process of creating an indexed image, may be better descripted as vector quantization. It’s sort of a rounding process for multidimensional numbers. More directly, all the colors in your image get grouped based upon their similarity. For a given group, all colors in that group are replaced by a single “center point” value, which minimizes error for colors in that cell. (or, “site” if you’re using the Voronoi terminology)

创建一个索引图像的过程形容起来比较像是一个矢量量化的过程。这是一种多维数字的舍入过程，更直接的说,所有的颜色将在你的图片中基于他们的相同之处得到分组。对于其中的任意一个分组，组中所有的颜色值将会被一个 “中心点” 的值所代替，最大限度的减少错误的颜色误差。

The image below shows this process for a 2D set of values.

以下的图片展示在一个 2D 的数组值中这个过程是如何进行的。

![image](https://cdn-images-1.medium.com/max/800/1*w5GkS92LEORhfT1_t3jygg.gif)

Green dots represent all input values in this 2D space. Blue lines represent “cells” or “clumps” of similar colors, and the red dots denote the representative color for that cell. The process of VQ for images means for every pixel, replacing it with the representative color for it’s VQ cell.

绿色的点代表 2D 空间中所有的输入值。蓝色的线代表相同颜色的“细胞”或是“团体”，红色的点表示这个“细胞”中主要的颜色。矢量量化的过程意味着对图像的每个像素,取而代之的是矢量量化的代表颜色。

The result of applying VQ to an image has the effect of reducing the number of unique colors, replacing it with a single color thats “pretty close” in visual quality.

对一个图片使用矢量量化的过程能够减少不同颜色值的数量，用一个单一的颜色来代替看起来十分相近的颜色。

It also has the ability to allow you to define the “maximum” number of unique colors in your image.

同样的也提供了设置不同颜色数量最大值的能力。

For example, the image below shows the 24-bit-per-pixel version of a parrot head, vs a version that only allowed 16 total unique colors to be used.

例如，下面这张图片显示了 24-bit-per-pixel 版本的鹦鹉头像，对比于一张只允许 16 种不同颜色的图像。

![image](https://cdn-images-1.medium.com/max/800/1*mZxHiZEee9u99jJZ9CMHHQ.png)

Immediately, you can see that there’s a loss of quality; most of the gradient colors have been replaced, giving the “banding” effect to the visual quality; obviously this image needs more than the 16 unique colors we gave it.

你能迅速的发现这过程中图片质量的损失，大部分的渐变颜色都被取代，使得实际看起来有 “条带” 的效果，很明显这张图片需要不止 16 种不同的颜色值来显示。

Setting up a VQ step in your pipeline can help you get a better sense of the true number of unique colors that your image uses, and can help you reduce them significantly. Sadly though, I don’t know of any image optimization tool out there that allows you to specify these values manually, other than pngquant. So, unless you’re using that tool, you might have to create your own VQ code to do this.

在使用图片之前加入一个图片矢量量化的过程能帮助你得到图片中使用的真正独特需要的颜色值，能显著的帮助你减小图片大小。不过,遗憾的是我不知道的任何图像优化工具,允许您手动指定这些值,除了pngquant。所以,除非你使用 pngquant ,否则你可能需要创建自己的矢量量化代码。

# It’s all about collaboration

# 协作是关键

The truth is, you should be using a tool to help you reduce the size of PNGs to as small as possible; The authors of these tools have spent a lot of time working on the issues, and it’s much faster for you to leverage their work. That being said, there’s still a lot of work that can be done by your artists (and yourself) to an image before sending it off to the extra programs to do their magic.

事实上，你应该使用一个工具来帮助你尽可能的减小 PNG 图片的大小，这些工具的作者已经花费了大量的时间来研究如何解决这些问题，直接利用他们的工作成果是十分便捷的。话虽这么说,还是有很多工作可以通过你自己来优化一个图像,然后再使用其他工具来实现他们的魔法。

So, go out there and make some smaller PNGs!

所以，让咱们的 PNG 图片越来越小吧！

HEY!

嘿！

Want to know how JPG files work, and how to make them smaller?

想知道 JPG 图片是如何工作的吗？想知道如何减小 JPG 图片的大小吗？

Want more data compression goodness? Buy my book!

想要更多的干货？请买我的书！
