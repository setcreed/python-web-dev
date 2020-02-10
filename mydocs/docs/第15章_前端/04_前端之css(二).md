# CSS属性相关

## 宽和高

- width属性可以为元素设置宽度
- height属性可以为元素设置高度
- 只有块级标签才可以设置长度，行内标签设置了也没有任何作用，只取决于文本内容大小

## 字体属性

### 文本颜色

- 直接写 颜色英文  color: red
- `#FF0000`   直接利用pycharm提供的取色器
- rgb(125,125,125)  获取三基色
- rgba(128,128,128,0.3)  最后一个数字只是用来调节颜色的透明度

### 文本字体

用font-family  控制字体种类

```css
body {
  font-family: "Microsoft Yahei", "微软雅黑", "Arial", sans-serif
}
```

### 字体大小

用font-size

```css
p {
  font-size: 36px;
}
```

### 字体粗细

font-weight

|   值    |                      描述                      |
| :-----: | :--------------------------------------------: |
| normal  |                默认值，标准粗细                |
|  bold   |                      粗体                      |
| bolder  |                      更粗                      |
| lighter |                      更细                      |
| 100~900 | 设置具体粗细，400等同于normal，而700等同于bold |

## 文字属性

### 文字对齐

text-align属性规定了元素的中文本的水平对齐方式

| 值     | 描述                   |
| :----- | ---------------------- |
| left   | 左对齐，默认就是左对齐 |
| right  | 右对齐                 |
| center | 居中对齐               |

### 文字装饰

text-decoration 属性用来给文字添加特殊效果

|      值      |           描述           |
| :----------: | :----------------------: |
|     none     |  默认。定义标准的文本。  |
|  underline   |   定义文本下的一条线。   |
|   overline   |   定义文本上的一条线。   |
| line-through | 定义穿过文本下的一条线。 |


注意： 一个应用场景就是 ：

```html
<style>
	a {
        text-decoration: none;
      }
</style>

<a href="https://www.baidu.com">首页</a>
```

原本a标签上内的文字有下划线，可以用`text-decoration: none; `去掉

### 首行缩进

将段落的第一行缩进 32像素：

```css
p {
  text-indent: 32px;
}
```

## 背景属性

```css
/*背景颜色*/
background-color: orange;

/*背景图片*/
background-image: url("123.png");

/*背景图片平铺排满整个页面*/
background-repeat: repeat;

/*背景图片不平铺*/
background-repeat: no-repeat;

/*背景图片只在水平方向上平铺*/
background-repeat: repeat-x;

/*背景图片只在垂直方向上平铺*/
background-repeat: repeat-y;

/*背景图片的位置*/
background-position: 20px 30px;
```

支持简写：`background:#336699 url('1.png') no-repeat left top;`

通常一个页面上的一个个的小图标并不是单独的，而是一张非常大的图片，该图片上有网址所用到的所有的小图标通过控制背景图片的位置，来显示不同的样式。

### 补充

```css
/*用来固定背景图片*/
background-attachment: fixed;
```





## 边框

边框属性 

分别写颜色、字体、样式

- border-width
- border-style
- border-color

```css
#d1 {
  border-width: 2px;
  border-style: solid;
  border-color: red;
}
```

通常使用简写方式：

```
#d1 {
  border: 2px solid red;
}
```

边框样式

|   值   |      描述      |
| :----: | :------------: |
|  none  |    无边框。    |
| dotted | 点状虚线边框。 |
| dashed | 矩形虚线边框。 |
| solid  |   实线边框。   |

## border-radius

能实现圆角边框的效果

```css
    <style>
        div {
            border: 1px solid blue;
            background-color: red;
            height: 200px;
            width: 200px;
            border-radius: 10px;
        }
    </style>
```

## display属性

```
inline 将块儿级标签变成行内标签
block  能够将行内标签 也能设置长宽和独占一行
inline-block;  /*即可以设置长宽 也可以在一行展示*/

display:none  隐藏标签 并且标签原来占的位置也没有了

visibility:hidden  隐藏标签 但是标签原来的位置还在 
```

## CSS盒子模型

- **margin**:            用于控制元素与元素之间的距离；margin的最基本用途就是控制元素周围空间的间隔，从视觉角度上达到相互隔开的目的。  <font color=red> 外边距</font>
- **padding**:           用于控制内容与边框之间的距离；  <font color=yellow>内边距(内填充)</font>  
- **Border**(边框):     围绕在内边距和内容外的边框。
- **Content**(内容):   盒子的内容，显示文本和图像。

### margin外边距

```css
.c1 {
  margin-top:5px;
  margin-right:10px;
  margin-bottom:15px;
  margin-left:20px;
}


/*可以简写*/
.c1 {
  margin: 5px 10px 15px 20px;
}

/*居中, 只能左右居中，不能上下居中*/
.c2 {
    margin: 0 auto;
}
```

顺序为：上右下左

### padding内边距

```css
.c3 {
  padding-top: 5px;
  padding-right: 10px;
  padding-bottom: 15px;
  padding-left: 20px;
}

/*简写*/
.c3 {
  padding: 5px 10px 15px 20px;
}
```

与margin一样，也是顺序为：上右下左

## float浮动

在css中，任何元素都可以浮动

浮动的元素是脱离正常文档流的（原来的位置会让出来）

浏览器会优先展示文本内容

```css
.c1 {
    height: 100px;
    width: 100px;
    background-color: red;
    float: left;   /*向左浮动*/
}
.c2 {
    height: 100px;
    width: 100px;
    background-color: green;
    float: right;  /*向右浮动*/
}
.c3 {
    height: 100px;
    width: 100px;
    background-color: blue;
    float: none;  /*默认值，不浮动*/
}
```

### 浮动造成的影响：

会造成父标签塌陷

## clear

clear属性能清除浮动带来的影响



| 值      | 描述                                  |
| ------- | ------------------------------------- |
| left    | 在左侧不允许浮动元素。                |
| right   | 在右侧不允许浮动元素。                |
| both    | 在左右两侧均不允许浮动元素。          |
| none    | 默认值。允许浮动元素出现在两侧。      |
| inherit | 规定应该从父元素继承 clear 属性的值。 |



清除浮动影响可以用 ：

```css
.clearfix:after {
    content: '';
    clear: both;   /*左右两边不能右浮动的元素*/
    display: block;
}

/*谁塌陷就把clearfix加给谁*/
```



## overflow溢出

| 值      | 描述                                                     |
| ------- | -------------------------------------------------------- |
| visible | 默认值。内容不会被修剪，会呈现在元素框之外。             |
| hidden  | 内容会被修剪，并且其余内容是不可见的。                   |
| scroll  | 内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。 |
| auto    | 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。 |




- overflow（水平和垂直均设置）
- overflow-x（设置水平方向）
- overflow-y（设置垂直方向

```css
/*设置圆形*/
overflow:hidden;
```

## 定位

所有的标签默认都是静态的，无法改变位置

position: static;
必须将静态的状态修改成定位之后

### 相对定位   relative

相对于标签原来的位置  移动

### 绝对定位   absolute

相对于已经定位过（只要不是static都可以）的父标签  再做定位

### 固定定位   fixed

相对于浏览器窗口固定在某个位置不动

### 脱离文档流

- 脱离文档流：绝对定位、固定定位
- 不脱离文档流：相对定位

## x-index

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .cover {
            background-color: rgba(128,128,128,0.5);
            position: fixed;
            top: 0;
            right: 0;
            bottom: 0;
            left: 0;
            z-index: 999;
        }
        .modal {
            background-color: white;
            position: fixed;
            /*top: 50%;*/
            /*left: 50%;*/
            z-index: 1000;
        }
    </style>
</head>
<body>
<div>我是最底下的被压着的那个</div>
<div class="cover"></div>
<div class="modal">
    <form action="">
        <p>username:
            <input type="text">
        </p>
        <p>password:
            <input type="password">
        </p>
        <input type="submit">
    </form>
</div>
</body>
</html>
```

![](https://i.loli.net/2019/11/14/KiVSAQNU5rTPxlt.png)



## 透明度

opacity

用来定义透明效果。取值范围是0~1，0是完全透明，1是完全不透明。