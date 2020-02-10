# python简介

Python的创始人为吉多·范罗苏姆（Guido van Rossum），1989年的圣诞节期间，Guido为了打发圣诞节的无聊时光，开始写能够解释Python语言语法的解释器。Python这个名字，来自Guido所挚爱的电视剧Monty Python’s Flying Circus。他希望这个新的叫做Python的语言，能符合他的理想：创造一种C和shell之间，功能全面，易学易用，可拓展的语言。



Python可以应用于众多领域，如：数据分析、组件集成、网络服务、图像处理、数值计算和科学计算等众多领域。目前业内几乎所有大中型互联网企业都在使用Python，如：Youtube、Dropbox、BT、Quora（中国知乎）、豆瓣、知乎、Google、Yahoo!、Facebook、NASA、百度、腾讯、汽车之家、美团等。

# python版本问题

python2.x 已经被抛弃了，以后统一使用python3

[python官网](https://www.python.org/)，现在python更新到 3.8了

# python解释器的类型

我们都知道Python是一门解释型语言，代码想运行，必须通过解释器执行，Python的解释器本身也可以看作是个程序，这个程序是什么语言开发的呢？ 答案是好几种语言？ 因为Python有好几种解释器，分别基于不同语言开发，每个解释器特点不同，但都能正常运行我们的Python代码，下面分别来看下各种不同类型的Python解释器的区别。

## CPython

CPython是使用最广且被的Python解释器。一般使用的是CPython。当我们从Python官方网站下载并安装好Python 3后，我们就直接获得了一个官方版本的解释器：CPython。这个解释器是用C语言开发的，所以叫CPython。在命令行下运行python就是启动CPython解释器。

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200128154036.png)

## IPython

IPython是基于CPython之上的一个交互式解释器，也就是说，IPython只是在交互方式上有所增强，但是执行Python代码的功能和CPython是完全一样的。

## PyPy

PyPy是另一个Python解释器，它的目标是执行速度。PyPy采用JIT技术，对Python代码进行动态编译（注意不是解释），所以可以显著提高Python代码的执行速度。

绝大部分Python代码都可以在PyPy下运行，但是PyPy和CPython有一些是不同的，这就导致相同的Python代码在两种解释器下执行可能会有不同的结果。

## Jython

Jython是运行在Java平台上的Python解释器，可以直接把Python代码编译成Java字节码执行。

## IronPython

IronPython和Jython类似，只不过IronPython是运行在微软.Net平台上的Python解释器，可以直接把Python代码编译成.Net的字节码。