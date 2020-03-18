# vue常见指令

## 表单指令

```
v-model="变量"  变量值与表单标签的value相关
v-model 可以实现数据的双向绑定： v-model绑定的变量值可以影响表单标签的值，反过来 单标签的值也可以影响变量的值

普通：变量就代表value的值
单选框：变量为多个单选框中的某一个value的值
单一复选框：变量为布尔类型，代表是否被选择
多复选框：变量为数组，存放的是选中的选项value
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
    <form action="">
        <div>
            <!--数据的双向绑定-->
            <input type="text" name="usr" v-model="v1">
            <input type="text" v-model="v1">
            {{ v1 }}
            <hr>

            <!--单选框-->
            男：<input type="radio" name="gender" value="male" v-model="v2">
            女：<input type="radio" name="gender" value="female" v-model="v2">
            {{ v2 }}
            <hr>

            <!--单一复选框-->
            卖身契：同意 <input type="checkbox" name="agree" v-model="v3">
            {{ v3 }}

            <!--多复选框-->
            爱好：<br>
            跑步：<input type="checkbox" name="hobbies" value="running" v-model="v4">
            读书：<input type="checkbox" name="hobbies" value="reading" v-model="v4">
            游泳：<input type="checkbox" name="hobbies" value="swimming" v-model="v4">
            {{ v4 }}

        </div>
    </form>
</div>
</body>
<script src="vue/vue.min.js"></script>
<script>
    new Vue ({
        el: '#app',
        data: {
            v1: '123',
            v2: 'male',
            v3: 'true',
            v4: ['running', 'reading', 'swimming']
        }
    })
</script>
</html>
```



## 条件指令

```
v-show="布尔变量"   隐藏时，采用display:none进行渲染
v-if="布尔变量"     隐藏时，不在页面中渲染 （可以保证不渲染的数据泄露）
v-if | v-else-if | v-else
```



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .box {
            width: 200px;
            height: 200px;
        }
        .r {background-color: red}
        .g {background-color: green}
        .b {background-color: blue}
        .active {
            background-color: purple;
        }
    </style>
</head>
<body>
<div id="app">
    <div class="box r" v-show="is_show"></div>

    <div class="box b" v-if="is_show"></div>
    <hr>

    <div class="wrap">
        <div class="box r" v-if="page === 'r_page'"></div>
        <div class="box g" v-else-if="page === 'g_page'"></div>
        <div class="box b" v-else></div>

        <div>
            <button @click="page='r_page'" :class="{active: page === 'r_page'}">红</button>
            <button @click="page='g_page'" :class="{active: page === 'g_page'}">绿</button>
            <button @click="page='b_page'" :class="{active: page === 'b_page'}">蓝</button>
        </div>
    </div>

</div>
</body>
<script src="vue/vue.min.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            is_show: false,
            page: 'r_page'
        }
    })
</script>
</html>
```






## 循环指令

```html
v-for="(v, i) in str|arry"
v-for="(v, k, i) in dic"
```



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
    <span>{{ info }}</span>
    <hr>

    <div v-for="c in info">{{ c }}</div>
    <hr>

    <div v-for="(c, i) in info">{{ i }}: {{ c }}</div>
    <hr>

<!--    <div v-for="(v, k, i) in people">{{ i }}  {{ k }}: {{ v }}</div>-->
    <hr>

    <div v-for="tea in teas">
        <span v-for="(v, k, i) in tea"><span v-if="i !== 0"> | </span>{{ k }}: {{ v }}</span>
    </div>
</div>
</body>
<script src="vue/vue.min.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            info: 'Keep quiet time for time',
            teas: [
                {
                    name: 'cwz',
                    age: 19,
                    gender: '男',
                },
                {
                    name: 'root',
                    age: 28,
                    gender: '女'
                },
                {
                    name: 'Reese',
                    age: 32,
                    gender: '男'
                }
            ]
        }
    })
</script>
</html>
```



## 循环指令案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        li:hover {
            color: red;
            cursor: pointer;
        }
    </style>
</head>
<body>
<div id="app">
    <input type="text" v-model="comment">
    <button type="button" @click="send_msg">留言</button>
    <ul>
        <li v-for="(msg, i) in info" @click="delete_msg(i)">{{ msg }}</li>
    </ul>
</div>
</body>
<script src="vue/vue.min.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            comment: '',
            info: localStorage.info ? JSON.parse(localStorage.info): []
        },

        methods: {
            send_msg() {
                // 将comment添加到 info数组中:  unshift push 首尾增 | shift pop 首尾删
                // this.info.unshift(this.comment)

                // 数组操作最全方法： splice(begin_index, count, ....args)
                // this.info.splice(0,0,this.comment)  // 首增
                // this.info.splice(0,1,this.comment)  // 替换
                // this.info.splice(0,1)  // 删除

                if (!this.comment) {
                    alert('请输入内容');
                    return false
                } else {
                    this.info.push(this.comment);
                    this.comment = '';

                }

                localStorage.info = JSON.stringify(this.info)

            },

            delete_msg(index) {
                this.info.splice(index, 1);
                localStorage.info = JSON.stringify(this.info)
            }
        }


    })
</script>
<script>
    // 前端数据库
    // localStorage：永久存储
    // sessionStorage: 临时存储（所属页面标签关闭后，清空）
    // localStorage.n1 = 10;
    // sessionStorage.n2 = 20;

    // localStorage.arr = JSON.stringify([1,2,3]);   // 只能存序列化后的数据
    // console.log(JSON.parse(localStorage.arr));

    localStorage.clear();  // 清空数据库

</script>
</html>
```



## 前台数据库

```python
前端数据库
localStorage：永久存储
sessionStorage: 临时存储（所属页面标签关闭后，清空）
localStorage.n1 = 10;
sessionStorage.n2 = 20;

localStorage.arr = JSON.stringify([1,2,3]);   // 只能存序列化后的数据
console.log(JSON.parse(localStorage.arr));

localStorage.clear();  // 清空数据库
```





## delimiters：分隔符

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
    {{ msg }}
</div>
</body>
<script src="vue/vue.min.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            msg: 'message'
        },
        delimiters: ['[{', '}]']   // 修改插值表达式符号
    })
</script>
</html>
```



## 过滤器  filters

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
    <p>{{ num | f1 }}</p>
    <p>{{ a, b | f2(30, 40) | f3 }}</p>
</div>
</body>
<script src="vue/vue.min.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            num: 10,
            a: 10,
            b: 20
        },

        filters: {
            // 传入所有要过滤的条件，返回值就是过滤的结果
            f1(num) {
                console.log(num);
                return num * 10;
            },

            f2(a, b, c, d) {
                console.log(a, b, c, d);  // 10 20 30 40
                return a + b + c + d;
            },

            f3(num) {
                return num * num
            }
        }

    })
</script>
</html>
```

总结：

- 在filters成员中定义过滤器方法
- 可以对多个值进行过滤，过滤时还可以传入额外的辅助参数
- 过滤的结果可以再进行下一次过滤 （过滤的串联）

## 计算属性  computed

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
    <input type="number" min="0" max="100" v-model="n1">
    +
    <input type="number" min="0" max="100" v-model="n2">
    =
    <button>{{ result }}</button>
</div>
</body>
<script src="vue/vue.min.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            n1: '',
            n2: '',

        },
        computed: {
            result() {
                console.log('被调用了');
                n1 = +this.n1;
                n2 = +this.n2;
                return n1 + n2;
            }
        }
    })
</script>
</html>
```

总结：

- `computed`计算属性可以申明 方法属性（方法属性一定不能在data中重复声明）
- 方法属性  必须在页面中渲染，才会启用绑定的方法，方法属性的值就是绑定方法的返回值
- 绑定的方法中出现的所有变量都会被监听，任何一个变化发生的值更新 都会重新触发绑定方法，从而更新方法属性的值

## watch 监听

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
    <p>姓名: <input type="text" v-model="full_name"></p>
    <p>姓：{{ first_name }}</p>
    <p>名：{{ last_name }}</p>
</div>
</body>
<script src="vue/vue.min.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            full_name: '',
            first_name: '未知',
            last_name: '未知',
        },

        watch: {
            full_name(n, o) {
                name_arr = n.split('');
                this.first_name = name_arr[0];
                this.last_name = name_arr[1];
            }
        }
    })
</script>
</html>
```

总结：

- 监听的属性需要在data中声明，监听方法不需要返回值
- 监听的方法名就是监听的属性名，该属性值发生更新时就会回调监听方法
- 监听方法有两个回调参数：当前值、 上一次值



## 冒泡排序

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">

</div>
</body>
<script src="vue/vue.min.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {}
    })
</script>
<script>

    let arr = [2, 43, 5, 67, 1, 3, 7, 10, 9];

    console.log(arr);

    for (let i = 0; i < arr.length - 1; i++) {
        for (let j = 0; j < arr.length - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp
            }
        }
    }
    console.log(arr)

</script>
</html>
```