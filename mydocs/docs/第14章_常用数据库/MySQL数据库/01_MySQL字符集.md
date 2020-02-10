# 字符集

- 字符集是一套符号和编码的规则,不论是在 oracle 数据库还是在 mysql 数据库,都存在字符集的选择问题。
- 而且如果在数据库创建阶段没有正确选择字符集,那么可能在后期需要更换字符集,而字符集的更换是代价比较高的操作,也存在一定的风险。
- 我们推荐在应用开始阶段,就按照需求正确的选择合适的字符集,避免后期不必要的调整。



## 查看所有的字符集

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200201200754.png)



在MySQL客户端和MySQL服务端之间存在着一个字符集转换器

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200201200930.png)



- character_set_client => utf8 (客户端告诉转换器发送过来的是utf8格式的编码)
- character_set_connection => utf8（将客户端传送过来的数据转换成utf8格式）
- character_set_results => utf8 (告诉客户端结果数据的编码是utf8)

注：以上三个字符集可以使用set names utf8来统一进行设置



## 用的字符集的详细信息



latin字符集

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200201201250.png)



gbk字符集

![image-20200201201337969](C:\Users\cwzdz\AppData\Roaming\Typora\typora-user-images\image-20200201201337969.png)



utf8字符集

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200201201419.png)

以上三幅图可以知道，每种字符集中，用于存储一个字符的最大的字节数目都不同，utf8最大，latin最
小。所以在经过字符集转换器的时候，如果处理不当，会造成数据丢失。



我们将character_set_connection的值改为lantin的时候

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200201201623.png)

从客户端发送过来的utf8数据，会被转成lantin1格式，因为utf8格式的数据占用的字符数较多，从而会造成数据丢失。

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200201201708.png)



避免方法：

就是数据在经过上面三层转换的时候，只要有一个数据的格式和它两个不一样的话，都会出现乱码，因此我们需要确保三者的字符集是一致的。

## 设置字符集的两种方法

- 使用set names utf8来统一设置，上文已说过
- 这种形式的设置只会在当前窗口有效，当窗口关闭后重新打开 CMD 客户端的时候又会出现乱码问题；那么，如何进行一个一劳永逸的设置呢？在 MySQL 的安装目录下有一个 my.ini 配置文件，通过修改这个配置文件可以一劳永逸的解决乱码问题。在这个配置文件中 [mysql] 与客户端配置相关，[mysqld] 与服务器配置相关。默认配置如下：

```bash
[mysql]
default-character-set=utf8
[mysqld]
character-set-server=utf8

```

