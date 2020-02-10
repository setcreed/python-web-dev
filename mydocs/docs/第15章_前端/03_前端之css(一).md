# 什么是css

css： 层叠样式表

css语法结构：

选择器 {属性1：属性值1}

# 注释

```css
/*单行注释*/

/*
多行注释1
多行注释2
*/
```

# CSS三种引入方式

- 通过link标签引入外部的css文件（最正规用法）

```html
<link rel="stylesheet" href="01%20css简介.css">
```

- 直接在html页面上的head内通过style标签直接书写css代码

```html
<style>
    h1 {
        color: red;
    }
</style>
```



- 行内标签（直接在标签内部通过style标签写）（不推荐使用）

```html
<h1 style="color: yellow">文本信息</h1>
```

# 基本选择器

## 元素选择器

![](https://i.loli.net/2019/11/13/fLZFboaHrD4hP8v.png)

```css
/*让所有的span标签变成红色*/

<style>
	span {
    	color: red;
	}
</style>
```



## id选择器

![](https://i.loli.net/2019/11/13/bFyOXsJhCLnMkxt.png)

```css
    <style>

        #d2 {
            color: gold;
        }
    </style>
```



## 类选择器

![](https://i.loli.net/2019/11/13/HAjg7WY6cZokfEC.png)

```css
    <style>
        .c1 {
            color: red;
        }
    </style>
```



## 通用选择器

选择所有

```css
    <style>
        * {
            color: orchid;
        }
    </style>
```



# 组合选择器

## 后代选择器

![](https://i.loli.net/2019/11/13/83BFpjVIkhKQ4mo.png)

## 儿子选择器

![](https://i.loli.net/2019/11/13/KpR58jJugcVdUSl.png)

## 弟弟选择器

![](https://i.loli.net/2019/11/13/RnjZhzExbOroc6B.png)



## 毗邻选择器

![](https://i.loli.net/2019/11/13/39IWeGhXC2m4yTc.png)

# 属性选择器

1. 找含有某个属性名的标签

![](https://i.loli.net/2019/11/13/pK6dV3iIbr5ZjeR.png)

2. 找含有某个属性名并且属性值是什么的标签

![](https://i.loli.net/2019/11/13/Lm7fvoNiy92x3OA.png)

3. 找p标签 含有某个属性名并且属性值是什么 的标签

![](https://i.loli.net/2019/11/13/mZzofnwI39BQGai.png)

- **任何的标签都有自己的默认的属性  id  class**
- **标签支持自定义任何多的属性（也就意味着可以让标签帮你携带一些额外的数据）**

# 分组与嵌套

分组

![](https://i.loli.net/2019/11/13/JIBcxU5ET4Xh3nF.png)

嵌套

![](https://i.loli.net/2019/11/13/A4QcesG6V1hkStP.png)

# 伪类选择器

a标签有四种状态

- 没有被点击过
- 鼠标悬浮在上面
- 点击之后不松手
- 点击之后 再回到原来的页面

我们将input框鼠标带你进去之后的那个状态叫做**input获取焦点**，

鼠标移出去之后的状态叫做**input框失去焦点**

```css
    <style>
        a:link {    /*没有点击为红色*/
            color: red;
        }
        a:hover {	/*鼠标悬浮之后为蓝色*/
            color: blue;
        }
        a:active {	/*点击之后不放手为黄色*/
            color: yellow;
        }
        a:visited {		/*点击之后回到原来的页面 显示为绿色*/
            color: green;
        }
    </style>
```

# 伪元素选择器

可以清除浮动带来的负面影响，可以通过css添加文本内容

## first-letter

常用的给首字母设置特殊样式：

```
p:first-letter {
  font-size: 48px;
  color: red;
}
```

## before

```
/*在每个<p>元素之前插入内容*/
p:before {
  content:"*";
  color:red;
}
```

## after

```
/*在每个<p>元素之后插入内容*/
p:after {
  content:"[?]";
  color:blue;
} 
```

![](https://i.loli.net/2019/11/13/4aNk8zCM57AIQJq.png)



# 选择器优先级

1. 选择器相同的情况下， 引入方式不同

   遵循就近原则， 谁离标签更近，就听谁的

2. 选择器不同的情况下

   行内选择器   >    id选择器    >     类选择器      >     元素选择器