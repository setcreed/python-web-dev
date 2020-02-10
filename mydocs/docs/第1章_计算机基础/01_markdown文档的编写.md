[TOC]
# markdown基本语法

## 标题

```
# 一级标题
## 二级标题
### 三级标题
```

## *斜体*

```
*斜体*
```



## **加粗**

```
**加粗**
```

## ==高亮==

```
==高亮==
```

## 上标2^3^

```
x^y^
```

效果： x^y^

## 下标 CH~4~

```
C~H~4
```

## 代码引用  

> hello world!



```
> hello world!
```

## 代码引入  （使用```+编程语言）

例子：

```python
print("hello world!")
```

## 插入链接（链接显示）

<https://www.cnblogs.com/setcreed/>

## 插入链接 （显示连接描述）

```
[我的博客园](https://www.cnblogs.com/setcreed/)
```



[我的博客](https://www.cnblogs.com/setcreed/)



## 插入图片（本地图片）

![刺客信条](1.jpg)

## 插入图片（网上链接）

![刺客信条](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1566388638499&di=1b2eb35bae8a40c922d4e3e60c85f056&imgtype=0&src=http%3A%2F%2Fgss0.baidu.com%2F-Po3dSag_xI4khGko9WTAnF6hhy%2Fzhidao%2Fpic%2Fitem%2F7dd98d1001e93901aa3a556d7fec54e736d196ad.jpg)

## 有序和无序列表

有序列表：

```
1. one
2. two
3. three
```

无序列表：

```
- one
- two
- three
```



- one
- two
- three

## 分割线 （---）

---

## 删除线

```
~~我被删除了~~
```

~~我被删除了~~



## 表格

**ctrl+/    进入源码模式**



```
姓名| 年龄| 身高
:-:|:-|-:
A|20|180
B|21|178
```



姓名| 年龄| 身高
:-:|:-|-:
A|20|180
B|21|178

## 数学公式

想要运用 Markdown 数学公式，必须在代码首尾加上这样一个符号`$` ，对其进行特殊标注。行内公式用`$代码$`

- $a+b$

上标采用`$^上标内容$`，下标采用`$_下标内容$`

- $x^2$
- $C_2H_5OH$

多个字符上标或下标用{}包裹作为一个单位

- $x^{2n}$
- $x_{2n}$

```
$\sum^{x \to \infty}_{y \to 0}{\frac{x}{y}}$
```

$\sum^{x \to \infty}_{y \to 0}{\frac{x}{y}}$

```
$\frac{1}{2}$
```

$\frac{1}{2}$

```
$\sum^{10}_{i=1}f(i)\,{thanks}$
```



$\sum^{10}_{i=1}f(i)\,{thanks}$

```
$$
\sum^{10}_{i=1}f(i)\,{thanks}
$$
```


$$
\sum_{i=1}^{10}f(i)\,{thanks}
$$

- 基础markdown语法参考：<https://www.appinn.com/markdown/>

- 插入数学公式markdown语法参考：https://www.zybuluo.com/codeep/note/163962