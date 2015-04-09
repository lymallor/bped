# PHP 扩展开发入门

关于 PHP 扩展开发的书，在市面上是否存在，我是不知道的。至少我还没见过中文版的。不过网上有几本国内高人们写的，或者翻译的国外高人写的电子书，其中我知道的，一本是：[深入理解PHP内核(Thinking In PHP Internals)](http://www.php-internals.com/)，另一本是：[PHP扩展开发及内核应用](http://www.walu.cc/phpbook/)，其中后一本好像还有其它一个或几个版本。另外还有著名的国内 PHP 大神[鸟哥的博客](http://www.laruence.com/)，[Swoole 开发大神韩天峰（Rango）的博客](http://rango.swoole.com/)，[黑夜路人的开源世界](http://blog.csdn.net/heiyeshuwu/article/category/193911)上也有一些关于 PHP 扩展开发的文章。我就是从读这些书和文章入门的。所以在这里我要首先感谢以上各位大神们能够提供这些十分优秀且珍贵的资源。

尽管已经有了上面这些资源，但是要开始编写扩展，感觉还是很难。一方面是上面这些书或文章的内容都比较深，像我这样的初学者，反正有一半内容是看不懂或者看不下去的（当然看不下去还有个原因是因为有些内容还用不到），另一方面是上面这些书或文章的内容有一部分是比较陈旧的，比如还有不少的篇幅是来介绍关于 PHP 4 扩展编写的。但是关于主流的 PHP 5.3 以上以及即将到来的 PHP 7 的内容，介绍的就比较少了（也不是说完全没有哈）。另外，关于实际开发中，会遇到的一些细枝末节的问题，介绍的也比较少，一方面可能大神们觉得这些内容是不值一提的，没有必要写，另一方面可能大神们已经对扩展的编写轻车熟路，所以也遇不到这些基础的小问题。

所以作为一个刚刚编写了两个小扩展，算是刚刚从 PHP 扩展编写的坑里爬出来的我，在这个时候做一下总结，我觉得还是有必要的。至少，或者说但愿，这些总结能够让我以后编写扩展的时候，尽量不再掉进这些坑里，或者掉进去之后能够更快的爬上来，而不至于再抓狂好几天。

大神们写的扩展，有很多都是关于操作 PHP 字节码或者对 PHP 本身开刀做手术的。编写这种扩展需要的知识很多，也很深。我是不懂的，所以我就不写了。有兴趣的同学可以去读读上面介绍的那些书和文章，或者读一读 PHP 源码，也许能找到你需要的东西。

本书[^1]的主要内容是以我所编写的两个扩展为例子，介绍 PHP 扩展开发中所遇到的一些实际问题以及解决方法，希望本书能够在帮助自己的同时，也能为想要学习 PHP 扩展开发的初学者们提供一些方便。

本书要讨论的两个 PHP 扩展一个是 xxtea，另一个是 hprose。

其中 xxtea 扩展是在多年之前就写好的，不过当时写的并不规范，直到最近发布到 pecl 上之后，才了解到编写一个规范的扩展需要注意哪些东西。我打算通过剖析这个扩展，来展示一下编写一个简单规范的扩展的流程。

而 hprose 扩展是多年之前就打算写，但直到最近才完成的一个扩展。这个扩展同样没有用到高深的知识，甚至连输入输出都没有涉及到，但是对于基础的东西基本都涉及到了。比如各种基本类型的操作，zval 变量的创建和销毁，HashTable，数组，类，接口，调用用户函数，方法和闭包，创建用户自定义类的对象，抛出异常，捕获异常等等。通过对这个扩展的剖析，我们基本上对 PHP 扩展开发中所遇到的基础问题可以有一个比较全面的了解。

这两个扩展既支持 PHP 5，也支持 PHP 7。所以本书也会把 PHP 5 和 PHP 7 扩展开发中的异同点，以及如何编写可以同时运行在这两个大版本上的扩展作为重点来讲解。

当然因为本人水平有限，加上 PHP 7 尚未发布，对一些内容的介绍上肯定会存在错误或漏洞，希望读者在谅解的同时，能够给予批评或指正。

本书暂未想好该如何规划结构，所以想到哪儿就先写到哪儿，对于写过的东西也会反反复复的修改，就像画画和编写程序一样。包括您已经看过的内容，也包括这一段。您不必为看过的东西可能在未来被删除而感到奇怪，如果您想找回它们，到 github 的历史记录里去翻查即可，这也是我为何要借助 github 来编写此书，而不是写在我的个人博客上的一个原因。

好了，废话不多说，因为已够多。下面就进入正文吧。

----
<small>[^1] 暂且称之为书吧，尽管才刚刚开始写，能写多少还不知道呢。</small>
