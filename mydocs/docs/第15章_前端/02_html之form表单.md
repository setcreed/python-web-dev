# form表单

表单能够获取用户输入，用于向服务器传输数据，从而实现用户与web服务器的交互

## 表单属性

## action	

控制数据提交的地址，有三种书写方式：

- 不写  默认就是朝当前这个页面所在的地址i提交数据
- 写全路径  如（https://www.baidu.com）
- 只写路径后缀（/index/）

## method    

控制数据提交的方式，有get、post

- form表单默认是get请求

## enctype

控制前端往后端提交数据的编码格式

 

## input标签

| type属性值 | 表现形式                                             |
| ---------- | ---------------------------------------------------- |
| text       | 单行输入文本                                         |
| password   | 密码输入显示  为密文形式                             |
| date       | 日期输入框，可以选择日期                             |
| radio      | 加上name，单选框                                     |
| button     | 普通按钮，没有实际意义，但是用的最多，可以绑定js事件 |
| reset      | 重置按钮                                             |
| submit     | 提交按钮，能够自动触发form表单提交数据的动作         |
| checkbox   | 复选框                                               |
| file       | 文本选择框                                           |
| hidden     | 隐藏输入框                                           |

**注意：能够触发form表单提交数据动作的标签有两个：**

- **input标签的submit  `<input type="submit" value="按钮">`**
- **button标签：`<button>我是一个button标签</button>`**

通常情况下input需要结合label一起使用

![](https://i.loli.net/2019/11/13/O3LgXdP9wtrfTal.png)

表单中的input元素根据type参数不同，展示不同的功能：

password

![](https://i.loli.net/2019/11/13/wCAcxNJpVZzHQj4.png)

date

![](https://i.loli.net/2019/11/13/qpkb4NuAlnfF1MQ.png)

radio  单选框

submit 提交按钮

button  普通按钮

reset重置按钮

![](https://i.loli.net/2019/11/13/QkTPAdeUy32zfHI.png)



![](https://i.loli.net/2019/11/13/WuNtX7aZIeOTEKh.png)



![](https://i.loli.net/2019/11/13/T785ucdlH63VBeZ.png)

注意：所有获取用户输入的标签  都应该有name属性

- **name属性就相当于字典的key**
- **input框获取到的用户输入  都会放到input框的value属性中**

input标签属性说明：

- name：<font color=red> name属性就相当于字典的key</font>
- value： 表单提交时对应的值，**input框获取到的用户输入  都会放到input框的value属性中**
- checked： `男 <input type="radio" name="gender" checked="checked">`  默认选中
- readonly： 只读模式，不能修改
- disabled:  禁用，所有input都适用

## select标签

```html
<p>
    省份:
    <select name="" id="">
        <option value="">上海</option>
        <option value="">北京</option>
        <option value="">广州</option>
        <option value="">深圳</option>
    </select>

</p>
<p>
    动漫
    <select name="" id="" multiple>
        <option value="">鬼刀</option>
        <option value="">灵笼</option>
        <option value="">风云</option>
    </select>
</p>
```

一个个的option标签就是一个个的选项

属性说明：

- multiple：布尔属性，设置后为多选，否则默认单选
- disabled：禁用
- selected：默认选中该项
- value：定义提交时的选项值



## textarea标签

多行文本

属性说明：

- name：名称
- rows：行数
- cols：列数
- disabled：禁用