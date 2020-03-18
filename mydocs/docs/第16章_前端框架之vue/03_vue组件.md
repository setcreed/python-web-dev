# vue组件

## 组件概念

html、css、js 的集合体，为该集合体命名，用该名字复用html、css与js组成的集合体    复用性

## 组件分类：

- 根组件：new Vue()  生成的组件
- 局部组件:  组件名={}  内部采用的是vue语法
- 全局组件： vue.component('组件名', {},  {})

## 组件的特点：

- 组件都有管理组件html 页面结果 的 template 实例成员，template中有且只有一个根标签
- 根组件都是作为最顶层父组件， 局部与全局组件作为子组件，也可以成为其他局部与全局的父组件
- 子组件的数据需要隔离（数据组件化，每一个组件拥有自己的数据的独立名称空间），data的值由方法的返回值提供
- 局部组件必须注册后才能使用，全局组件不需要注册，提倡使用局部组件
- 组件中出现的所有的变量，都由该组件自己提供管理
- 局部全局和根组件都是一个vue实例，一个实例对应一套html、css、js结构，所以实例就是组件



## 根组件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
    <h1>{{ msg }}</h1>    
</div>
</body>
<script src="vue/vue.min.js"></script>
<script>
    new Vue({
        el: '#app', // 挂载点不是虚拟DOM，而是要被虚拟DOM 替换的对象
        			// 被组件 template 模块尽行替换的占位符
        data: {
            msg: '根组件'
        },

        template: '<i>显示模板</i>'
    })
</script>
</html>
```

挂载点 本质上是被组件 template 模块进行替换的占位符

总结：

- 根组件 可以不明确template，template默认采用挂载点 页面结构，如果设置的template，挂载点内部的内容无效 

## 局部组件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
<div id="app">
    <local-tag></local-tag>
    <local-tag></local-tag>
</div>
<script>
    let localTag = {
        data () {
            return {
                count: 0
            }
        },
        template: '<button @click="btnAction">局部{{ count }}</button>',
        methods: {
            btnAction () {
                this.count ++
            }
        }
    }
    new Vue({
        el: "#app",
        components: {
            'local-tag': localTag
        }
    })
</script>
```



## 全局组件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
    <global-tag></global-tag>
    <global-tag></global-tag>
</div>
<script>
	Vue.component('global-tag', {
		data () {
			return {
				count: 0
			}
		},
		template: '<button @click="btnAction">全局{{ count }}</button>',
		methods: {
			btnAction () {
				this.count ++
			}
		}
	})
    new Vue({
        el: "#app"
    })
</script>
</html>
```



## 数据的组件化

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body, h2 {
            margin: 0;
        }
        .wrap {
            width: 880px;
            margin: 0 auto;
        }
        .wrap:after {
            content: '';
            display: block;
            clear: both;
        }
        .box {
            width: 200px;
            border-radius: 10px;
            overflow: hidden;
            background-color: darkgrey;
            float: left;
            margin: 10px;
        }
        .box img {
            width: 100%;
        }
        .box h2 {
            text-align: center;
            font-weight: normal;
            font-size: 20px;
        }
    </style>
</head>
<body>
<div id="app">
    <div class="wrap">
        <local-tag></local-tag>
        <local-tag></local-tag>
        <local-tag></local-tag>
        <local-tag></local-tag>

        <global-tag></global-tag>
        <global-tag></global-tag>
        <global-tag></global-tag>
        <global-tag></global-tag>


     </div>
</div>
</body>
<script src="vue/vue.min.js"></script>
<script>
    let localTag = {
        // 声明局部组件，局部组件要在其父组件中注册才能使用
        template: `
        <div class="box" @click="fn">
            <img src="img/img123.jpg" alt="">
        <h2>阿斯弗 {{ count }}</h2>
        </div>
        `,

        data () {   // 局部或全局组件，一个模板可能会被复用多次，每个组件都应该有自己独立的变量名称空间
          return {
              count: 0
          }
        }, // 数据需要组件化，作为方法的返回值（方法执行后会产生一个局部作用域）

        methods: {
            fn() {
                console.log(this);
                this.count ++
            }
        }

    };

    Vue.component('global-tag', {
        template: `
        <div class="box" @click="fn">
            <img src="img/img2.jpg" alt="">
        <h2>潜水</h2>
        `,

        methods: {
            fn() {
                console.log(this)
            }
        }

    });

    new Vue({
        el: '#app',
        data: {},
        components: {
            localTag: localTag
        }
    })
</script>
</html>
```



## 父组件传递数据给子组件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body, h2 {
            margin: 0;
        }
        .wrap {
            width: 880px;
            margin: 0 auto;
        }
        .wrap:after {
            content: '';
            display: block;
            clear: both;
        }
        .box {
            width: 200px;
            border-radius: 10px;
            overflow: hidden;
            background-color: darkgrey;
            float: left;
            margin: 10px;
        }
        .box img {
            width: 200px;
        }
        .box h2 {
            text-align: center;
            font-weight: normal;
            font-size: 20px;
        }
    </style>
</head>
<body>
<div id="app">
    <div class="wrap">
        <local-tag v-for="view in views" :view="view"></local-tag>


     </div>
</div>
</body>
<script src="vue/vue.min.js"></script>
<script>

    let views = [
        {
            name: '1号',
            img: 'img/100.jpg'
        },
        {
            name: '2号',
            img: 'img/200.jpg'
        },
        {
            name: '3号',
            img: 'img/300.jpg'
        },
        {
            name: '4号',
            img: 'img/400.jpg'
        }
    ];

    // 子组件可以通过props自定义组件属性  （采用反射机制，需要填写字符串，但是使用时可以直接作为变量）
    // 子组件会在父组件中渲染，渲染时将父组件的变量绑定给子组件的自定义属性，就可以将变量值传递给子组件
    let localTag = {
        props: ['view'],

        // 声明局部组件，局部组件要在其父组件中注册才能使用
        template: `
        <div class="box" @click="fn">
            <img :src="view.img" alt="">
        <h2>{{ view.name }} {{ count }}</h2>

        </div>
        `,

        data () {   // 局部或全局组件，一个模板可能会被复用多次，每个组件都应该有自己独立的变量名称空间
          return {
              count: 0
          }
        }, // 数据需要组件化，作为方法的返回值（方法执行后会产生一个局部作用域）

        methods: {
            fn() {
                console.log(this.view);
                this.count ++
            }
        }

    };


    new Vue({
        el: '#app',
        data: {
            views
        },
        components: {
            localTag: localTag
        }
    })
</script>
</html>
```

总结：

- 子组件可以通过props自定义组件属性 （采用反射机制，需要填写字符串，但是使用时可以直接作为变量）
- 子组件会在父组件中渲染，渲染时，将父组件的变量绑定给子组件的自定义属性，就可以将变量值传递给子组件。

## 子组件传递数据给父组件

自定义组件标签的事件：

- 事件是属于子组件的，子组件在父组件中渲染并绑定事件方法，所以事件方法由父组件来实现

子组件如何触发自定义事件：`this.$emit('自定义事件名', '触发事件回调的参数')`

子组件触发自定义事件，携带出子组件的内容，在父组件中实现自定义事件的方法，拿到子组件传递给父组件的消息



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
    <h1>{{ h1 }}</h1>
    <h2>{{ h2 }}</h2>
    <tag @action="actionFn"></tag>
    <hr>
    <tag2 @h1a="aFn1" @h2a="aFn2"></tag2>

</div>
</body>
<script src="vue/vue.min.js"></script>
<script>
    let tag = {

        template: `
      <div>
        <input type="text" v-model="t1">
        <input type="text" v-model="t2">
        <br>
        <button @click="changeTitle">修改标题</button>

      </div>
      `,

        data() {
            return {
                t1: '',
                t2: '',
            }
        },

        methods: {
            changeTitle() {

                if (this.t1 && this.t2) {
                    // console.log(this.t1, this.t2);
                    this.$emit('action', this.t1, this.t2);
                    this.t1 = '';
                    this.t2 = '';
                }
            }
        }

    };

    let tag2 = {
      template: `
      <div>
        主标题<input type="text" v-model="t3" @input="t1Fn">
        子标题<input type="text" v-model="t4" @input="t2Fn">
       </div>
      `,

        data() {
          return {
              t3: '',
              t4: '',
          }
        },

        methods: {
            t1Fn() {
                this.$emit('h1a', this.t3)
            },
            t2Fn() {
                this.$emit('h2a', this.t4)
            }
        }

    };


    new Vue({
        el: '#app',
        data: {
            h1: '主标题',
            h2: '子标题'
        },
        components: {
            tag,
            tag2
        },
        methods: {
            actionFn(a, b, c) {
                // console.log('触发了', a, b, c)
                this.h1 = a;
                this.h2 = b;
            },

            aFn1(a) {
                if (!a) {
                    this.h1 = '主标题';
                    return
                }
                this.h1 = a
            },
            aFn2(a) {
                if (!a) {
                    this.h2= '子标题';
                    return
                }
                this.h2 = a
            }
        }
    })
</script>
</html>
```
