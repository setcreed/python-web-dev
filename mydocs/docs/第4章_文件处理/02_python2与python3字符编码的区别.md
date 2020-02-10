# 执行python程序的三个阶段

准备一个`test.py`文件测试，文件内容是以`utf8`的格式保存的

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200131151259.png)

## 阶段一： 

启动python解释器，python解释器相当于一个文本编辑器，把`test.py`中文件内容从硬盘读取到内存中。如果`test.py`文件中 不指定读取的编码格式，那么python就会使用默认的编码格式读取内容。

查看python2与python3默认使用的字符编码

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200131151321.png)

使用`sys.getdefaultencoding()`获取各自的编码格式，发现python2默认用ascii码读取字符，而python3默认用`utf-8`读取字符。

不指定编码格式，直接分别用python2和python3读取其中的内容：

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200131151350.png)

指定编码格式为`utf8`，读取：

```python
#coding:utf8
你好啊
```



![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200131151418.png)

总结：

- 对于Python2来讲，python解释器在读取到中文字符的字节码时，会先查看当前代码文件头部是否指明字符编码是什么。如果没有指定，则使用默认字符编码`ASCII`进行解码，导致中文字符解码失败。
- 对于python3来说，过程是一样的，只不过python3的解释器以`utf-8`作为默认编码，但这并不能兼容中文字节的问题。在windows中，可能文件保存为`gbk`的形式存入硬盘中。python3解释器执行该代码时，会试图用`utf-8`来解码，那肯定会出错。

## 阶段二

读取已经加载到内存的代码，然后执行，执行过程中可能会开辟新的内存空间，比如说`msg = 'hello'`,

在程序执行前，内存中确实是以`Unicode`的格式放于内存中的。但是在程序执行过程中，会申请内存空间存放python的数据类型的值。

比如说`msg = 'hello'`，会被python解释器识别为字符串，会申请内存空间来存放字符串类型的值，至于该字符串类型的值被识别成何种编码存放，这就与Python解释器的有关了，而Python2与Python3的字符串类型又有所不同。



![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200131151452.png)

## 阶段三

将读取的内容打印到终端，若终端的编码格式是utf-8： 

- 在Python2中如果指定了字符编码，那么内存存取就会按照指定的字符编码去入内存。解释或去执行时就要按照指定了的字符编码去解释，否则就会乱码。 否则可以在定义变量前面加上u，如`u('中')`，这样变量就会以unicode编码存入内存。
- 但在Python3中就不会有这样的问题，因为无论你指定了什么字符编码，都会使用Unicode编码去存入内存，Unicode编码可以和任意的字符编码相互转换，并在读取时按照所需的编码去读取，这样就很好解决了字符编码的问题

# python2与python3 字符串类型的区别

## python2字符串类型

```python
class basestring(object):
    pass

class str(basestring):
    pass

class unicode(basestring):
    pass
```

`str `和 `unicode` 都是 `basestring`的子类。严格意义上来说，str是字节串，是由unicode经过编码后的字节组成的序列，对utf-8编码的“中”字使用`len()`函数，得到结果为3，这是因为utf-8编码的“中”为`\xe4\xb8\xad`。

unicode才是严格意义上的字符串，对字节串str使用正确的字符编码进行解码后获得，并且`len(u'中')==1`

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200131151542.png)



## python3字符串类型

python3中对字符串的支持进行了实现类层次的上简化，去掉了unicode类，添加了一个bytes类。从表面上来看，可认为python3中的str和unicode合二为一了。

```python
class bytes(object):
	pass


class str(object):
    pass
```

与python2不同，python3中的str已经是真正的字符串，而字节是用单独的bytes类来表示。也就是说python3默认的就是字符串，实现了对Unicode的内置支持，减轻了程序员对字符串处理的负担。



![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200131151609.png)

## 两者比较总结

对于单个字符的编码，python提供了`ord()`函数把字符转化为整数，`chr()`函数把整数转化为相应的字符

```python
print(ord('中'))   # 20013

print(chr(20013))  # 中
```



- python2默认用ascii 读取字符，python3默认用utf-8读取字符，可以用coding头部指定读取编码的格式
- 字节串(Python2的str默认是字节串)-->decode('原来的字符编码')-->Unicode字符串-->encode('新的字符编码')-->字节串

```python
#!/usr/bin/env python2
#-*- coding:utf-8 -*- 
utf_8_a = '中国'
gbk_a = utf_8_a.decode('utf-8').encode('gbk')
print(gbk_a.decode('gbk'))
# 输出结果：中国
```

- 字符串(str就是Unicode字符串)-->encode('新的字符编码')-->字节串

```python
#!/usr/bin/env python3
#-*- coding:utf-8 -*- 
utf_8_b = '中国'
gbk_b = utf_8_b.encode('gbk')
print(gbk_b.decode('gbk'))
# 输出结果：中国
```



![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200131151635.png)

**总之：用什么编码存，就用什么编码取。**