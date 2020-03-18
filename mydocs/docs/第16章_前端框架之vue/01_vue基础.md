# vue基础

渐进式JavaScript 框架

安装[vue](https://cn.vuejs.org/)       

## 一、vue简介

### 1、什么是vue

前端三大框架之一， 可以独立完成前后端分离式web项目的JavaScript框架

### 2、vue的优势

- 前端三大主流框架： Angular    React      Vue
- 先进的前端设计模式： MVVM （Model-View-ViewModel）
- 中文API、可以单页面、组件化开发

### 3、特点

```
单页面web应用
数据驱动
数据的双向绑定
虚拟DOM
```

### 4、使用vue

- 开发版本：[vue.js](https://vuejs.org/js/vue.js)
- 生产版本：[vue.min.js](https://vuejs.org/js/vue.min.js)

```html
<div id="app">
	{{ }}
</div>
<script src="js/vue.min.js"></script>
<script>
	new Vue({
		el: '#app'   // 挂载点，vue实例与页面标签建立关联
	})
</script>
```

## 二、Vue实例

### 1、el：实例

```js
new Vue({
        el: '#app',
});
```



![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200306165901.png)

总结：

- 实例与页面挂载点一一对应
- 一个页面中可以出现多个实例对应的挂载点
- 通常挂载点都采用id选择器
- html与body标签不能作为挂载点

### 2、data  数据

```html
<div id="app">
    {{ msg }}
</div>


<script>
    var app = new Vue({
        el: '#app',
        data: {
            msg: '数据',
        }
    });

    console.log(app.msg);
    console.log(app.$data.msg)
</script>
```

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200306170117.png)

### 3、methods   方法

```html
<div id="app">
    <p v-bind:style="pStyle" v-on:click="pClick">{{ msg }}</p>
</div>


<script>
    var app = new Vue({
        el: '#app',
        data: {
            msg: '测试',
            pStyle: {
                color: 'blue'
            }
        },

        methods: {
            pClick () {
                if (app.pStyle.color !== 'blue') {
                    app.pStyle.color = 'blue'
                }else {
                    app.pStyle.color = 'red'
                }

                console.log(app.msg);
                console.log(this.msg);
                console.log(app.pClick);
                console.log(this.pClick)
            },

        }
    })

</script>
```

声明的实例是否用一个变量接收

- 在实例内部不需要，用this就代表vue实例本身
- 在实例外部或其他实例内部就需要， 定义一个变量接收new Vue()产生的实例

## 三、插值表达式

```html
<body>
<div id="app">
    <p>{{ msg }}</p>
    <p>{{ num + msg }}</p>
    <p>{{ num * 10 }}</p>
    <p>{{ msg[0] }}</p>
    <p>{{ msg.split('') }}</p>
    <p></p>
</div>
</body>
<script src="vue/vue.min.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            msg: '信息',
            num: 10
        }
    })
</script>
```

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200306170145.png)



## 四、js面向对象补充

```html
<script>
    function f1() {
        console.log('from f1')
    }
    f1();

    // 构造函数 == 类
    function F2(name) {
        this.name = name;
        this.eat = function (food) {
            console.log(this.name + '吃' + food)
        }
    }
    let ff1 = new F2("reese");
    console.log(ff1.name);

    ff1.eat('苹果');


    let obj = {
        name: 'cwz',
        // eat: function (food) {
        //     console.log(this.name + '吃' + food)
        // }

        eat (food) {   // 方法的简写
            console.log(this.name + '吃' + food)
        }
    };

    console.log(obj.name);
    obj.eat('水果')


</script>
```



## 五、js函数补充

```html
<script>
    function f() {
        var a = 1;
        let b = 2;
        const c = 3;
        d = 4;
    }

    f();
    // console.log(a);    // let、const定义的变量不能重复定义，且具备块级作用域
    // console.log(b);
    // console.log(c);
    console.log(d)

</script>
```



箭头函数

```js
	function f() {
        var a = 1;
        let b = 2;
        const c = 3;
        d = 4;
    }

    f();
    // console.log(a);    // let、const定义的变量不能重复定义，且具备块级作用域
    // console.log(b);
    // console.log(c);
    // console.log(d)


    function f1() {
        console.log('from f1')
    }
    f1();

    let f2 = () => {
        console.log('from f2')
    };
    f2();

    // 如果箭头函数没有函数体，只有返回值的情况
    let f3 = (x, y) => x + y;
    let res = f3(10, 20);
    console.log(res);

    // 如果箭头函数参数列表只有一个，可以省略括号
    let f4 = num => num + 100;
    res1 = f4(10);
    console.log(res1)
```

function、箭头函数、方法都具有本质区别

```js
let obj = {
        name: 'cwz',
        eat: function (food) {
            console.log(this);   // {name: "cwz", eat: ƒ, eat2: ƒ}
            console.log(this.name + '在吃' + food)
        },

        eat2: food => {
            console.log(this);  // Window {parent: Window, postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, …}
            console.log(this.name + '在吃' + food)
        }

    };
obj.eat('阿达发');
obj.eat2('雪花')
```

箭头函数内部是没有`this`的，这个`this`不能指向当前对象，只能往上找



## 六、vue指令

### 1、文本指令

- {{  }}
- v-text  ：不能解析html语法，会原样输出
- v-html：能解析html语法的文本
- v-once：处理的标签的内容只能被解析一次

```html
<body>
<div id="d1">
    <p>{{ msg.split('') }}</p>
    <p v-text="msg.split('')"></p>
    <p v-text="info"></p>
    <p v-html="info"></p>

</div>
</body>
<script src="vue/vue.min.js"></script>
<script>
    new Vue({
        el: '#d1',
        data: {
            msg: '123',
            info: '<i>info</i>'
        }
    })
</script>
```

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200306170240.png)



### 2、事件指令

```html
v-on: 事件名="方法变量"  可以简写成  @事件名="方法变量"
<p @click="fn"></p>

事件变量，不添加()，默认会传事件对象: $event
<p @click="fn()"></p>

事件变量，添加()，代表要自定义传参，系统不再传入事件对象，但是可以手动传入事件对象
<p @click="fn($event)"></p>
```



```html
<div id="app">
    <p @click="f1">{{ msg }}</p>
    <hr>

    <p @mouseover="f2" @mouseout="f3" @mouseup="f4" @mousedown="f5" @mousemove="f6" @contextmenu="f7">{{ action }}</p>
    <hr>

    <p @click="f8($event, '第一个')">{{ info }}</p>
</div>
</body>
<script src="vue/vue.min.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            msg: '点击切换',
            action: '鼠标事件',
            info: '点击'
        },

        methods: {
            f1 () {
                this.msg = '点击了'
            },
            f2 () {
                this.action = '悬浮'
            },
            f3 () {
                this.action = '离开'
            },
            f4 () {
                this.action = '按下'
            },
            f5 () {
                this.action = '抬起'
            },
            f6 () {
                this.action = '移动'
            },
            f7 () {
                this.action = '右键'
            },

            f8 (ev, argv) {
                console.log(ev, argv);
                this.info = argv + '点击了'
            }
        }
    })
</script>
```





### 3、属性指令

格式：

```
v-bind: 属性名="变量"
简写为： :属性名="变量"
```





```html
单类名绑定
<p v-bind:class="c1"></p>

多类名绑定用[]语法， 采用多个变量来控制
<p v-bind:class="[c2, c3]"></p>

类名状态绑定, 类名：布尔值， 控制某类名是否器作用
<p v-bind:class="{c4: true|false}"></p>   如果是false就失效；如果是true，就起作用

多类名状态绑定
<p v-bind:class="[{c5: true}, {c6: flase}]"></p>

style属性绑定
<p :style="myStyle">样式属性</p>
<p :style="{width: w,height: h, backgroundColor: bgc}">样式属性</p>
```




