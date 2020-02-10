# 什么是前端

简单来说，任何直接与用户打交道的操作界面都叫前端。

# 前端基础

html  ---> 骨架

css  ----> 衣服、打扮

JavaScript  ----> 动态效果

- html     标签
- css      属性、选择器
- JavaScript    BOOM & DOOM

# 前端必备知识点

## 软件开发架构

c/s 

b/s  本质上也是c/s

## web服务的本质

客户端向服务端请求，

服务端向客户端相应

当在浏览器地址栏输入网址，回车会发生什么事情：

1. 向指定的服务器地址（也就是IP+PORT）发送请求
2. 服务器接受请求，并处理
3. 服务器响应浏览器的请求
4. 浏览器接收到，并渲染出好看的界面供用户观看

```python
import socket

server = socket.socket()
server.bind(('127.0.0.1', 9527))
server.listen(5)

while True:
    conn, addr = server.accept()
    data = conn.recv(1024)
    conn.send(b'HTTP/1.1 200 ok\r\n\r\n')
    conn.send('hello'.encode('utf-8'))
    conn.close()
```



## 请求方式

- get     向服务器要数据     如 ：输入百度网址，浏览器返回的是百度首页
- post    向服务器提交数据    如： 登录功能

# HTTP协议

## 是什么

超文本传输协议，规定了浏览器与服务端数据传输的数据格式。

## 四大特性

1. 基于TCP/IP，作用于应用层之上的协议
2. 基于请求  响应
3. 无状态
   - 不保存客户端状态
   - 无论来多少次，都当你如初见
4. 无连接
   - http连接，请求发过来立即给客户端相应，之后就与客户端断掉了。
   - 长连接，用websocket 做聊天室

## 数据格式

**请求格式**

```
请求首行（请求方式  协议版本）
请求头（一大堆k:v键值对）

请求体（敏感信息 密码 身份证号码）
```

**注意：请求头下面有一行空格，\r\n**

**响应格式**

```
响应首行（响应方式 协议版本）
响应头（一大堆k:v键值对）

响应体（给用户看的数据）
```

## 响应状态码

就是用数字来表示一串文字需要表达的意思

- 1xx：服务器已经成功接收到了请求，正在处理，你可以继续提交其他数据
- 2xx：服务器成功响应了相应的数据。如200
- 3xx：重定向。如访问一个需要登录才能访问的网页，就会跳转到登录页面
- 4xx：如404表示请求的资源不存在，403表示当前不符合某一些条件，无法正常访问
- 5xx：服务器内部错误  （500）

# HTML是什么

HTML全称为超文本标记语言（Hyper Text Markup Language），是一种创建网页的标记语言

# HTML文档结构

最基本的HTML文档

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>标题</title>
</head>
<body>

</body>
</html>
```

- `<!DOCTYPE html>` 声明为HMTL5

- `<html> </html>`是文档开始结束的标记
- `<head> </head>`定义了HTML文档开头的部分，之间的内容不会在浏览器的文档窗口。
- `<title> </title>`定义了网页标题
- `<body> </body>`是文本的主体内容

# HTML的注释

```html
<!--单行注释-->

<!--
多行注释1
多行注释2
-->
```



# 标签的分类

## 双标签和自闭合标签

双标签：h1   a

自闭合标签：img

## 块级标签和行级标签

块级标签：h1~h6	p	br	hr	div   独占一行

-  块级标签内可以嵌套其他块级标签和行内标签
- p标签虽然是块级标签，但它内部不能嵌套任何块级标签，只能嵌套行内标签

行内标签     s    i    u    b    span

内部文本多大，就占多大

行内不能嵌套行内和块级标签

# 标签属性

- id 每一个标签都应该有id值，并且在同一个html文档中，id值不能重复
- class   类属性   



# HTML常用标签

## head内常用标签

| 标签                | 意义                                       |
| :------------------ | ------------------------------------------ |
| `<title></title>`   | 定义网页标题                               |
| `<style></style>`   | 内部支持写css代码                          |
| `<script></script>` | 内部可以直接写js代码，也可以引入外部js代码 |
| `<link/>`           | 引入外部css样式文件                        |
| `<meta/>`           | 定义网页源信息                             |

meta标签 内部keyword关键字提高被搜索引擎搜索的概率

## body内常用标签

| 标签  | 意义     |
| ----- | -------- |
| h1~h6 | 标题标签 |
| p     | 段落标签 |
| s     | 删除线   |
| u     | 下划线   |
| i     | 斜体     |
| br    | 换行     |
| hr    | 分割线   |
| b     | 加粗     |

## body内特殊符号

| 符号     | 意义 |
| -------- | ---- |
| `&nbsp;` | 空格 |
| `&amp;`  | &    |
| `&yen;`  | ￥   |
| `&gt`;   | >    |
| `&lt;`   | <    |
| `&copy;` | ©    |
| `&reg;`  | ®    |

## body内重要的标签

### div和span标签

div  就是一块区域

span 用来定义内联(行内)元素，并无实际的意义。主要通过CSS样式为其赋予不同的表现。

div和span是前期构建网页的基本骨架

### 链接标签a

1. 跳转功能  

```html
<a href="https://www.baidu.com" target="_blank">首页</a>
```

href 参数控制跳转的地址，target参数： target = '`_blank`' 表示新建窗口打开；target = '`_self`' 在当前窗口跳转

2. 锚点功能

给a标签设置id值, 然后在href中书写对应的a标签id值 点击即可跳转到对应的位置

```html
<h1 id="h1">我是标题1</h1>

<a href="#h1">首页</a>
```

### img图片标签

```html
<img src="3.jpg" alt="这里是图片" title="美化" width="200">
```

- src 是图片地址

- alt  当图片加载不出来时展示的信息

- width和height   这两个参数 你只需要设置一个 就可以   默认是等比例缩放

### 列表标签

- 无序列表

```html
<ul type="circle">
    <li>内容1</li>
    <li>内容2</li>
    <li>内容3</li>
</ul>



type参数
    disc（实心圆点，默认值）
    circle（空心圆圈）
    square（实心方块）
    none（无样式）
```



- 有序列表

```html
<ol type="a">
    <li>内容1</li>
    <li>内容2</li>
    <li>内容3</li>
</ol>


type参数
    1 数字列表，默认值
    A 大写字母
    a 小写字母
    Ⅰ大写罗马
    ⅰ小写罗马
```



### 标题列表

```html
<dl>
    <dt>
        标题1
    </dt>
    <dd>
        章节1
    </dd>
        <dt>
        标题2
    </dt>
    <dd>
        章节2
    </dd>
</dl>
```



### 表格标签

展示数据

```html
<table border="1">
    <thead>
    <tr>
        <th>user</th>
        <th>pwd</th>
        <th>age</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>neo</td>
        <td>123</td>
        <td>20</td>
    </tr>
    <tr>
        <td>jason</td>
        <td>qwe</td>
        <td>18</td>
    </tr>
    </tbody>
</table>
```

属性:

- border: 表格边框.
- cellpadding: 内边距
- cellspacing: 外边距.
- width: 像素 百分比.（最好通过css来设置长宽）
- rowspan: 单元格竖跨多少行
- colspan: 单元格横跨多少列（即合并单元格）

