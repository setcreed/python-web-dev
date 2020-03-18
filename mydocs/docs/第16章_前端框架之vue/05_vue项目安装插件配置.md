# vue项目安装插件配置

## vue安装ajax插件：axios

- 安装插件  在项目目录下安装

```
cnpm install axios
```

- 在main.js中配置

```js
import axios from 'axios'
Vue.prototype.$axios = axios
```

- 在一个组件的逻辑中发送ajax请求

```js
  // 完成ajax请求后台，获取数据库中的数据
  this.$axios({
  	url: this.$settings.base_url + '/cars/',
  	method: 'post',
  	params: {  // url拼接参数：任何请求都可以携带
  		a:1,
  		b:2,
  		c:3
  	},
      headers: {  //  不能随便携带数据, 必须是authorization
          authorization: '1234'
      },
  	data: {   // 数据包参数：get请求是无法携带的
  		x: 10,
  		y: 20
  	},
      
  }).then(response => {
  	// console.log(response);
  	this.cars = response.data;
  }).catch(error => {
  	console.log(error);
  })
```

- headers 补充

```python
# 后端拿headers数据 在 request.META.get('HTTP_参数名大写')
print(request.META.get('HTTP_AUTHORIZATION'))
  
  
# 如果要自定义headers参数, 可以在settings文件中配置
  CORS_ALLOW_HEADERS = [
      "accept",
      "accept-encoding",
      "authorization",
      "content-type",
      "dnt",
      "origin",
      "user-agent",
      "x-csrftoken",
      "x-requested-with",
      # 前端自定义参数, 如:
      "token",
  ]
```

  

## CORS跨域问题（同源策略）

同源：http协议相同、服务器ip地址相同、app应用端口相同

跨域：协议、ip地址、应用端口有一个不同，就是跨域

django默认是同源策略，存在跨域问题。

解决方案：

- django安装cors模块：

```
pip install django-cors-headers
```

- 在settings文件中注册模块，配置中间件：

```python
  INSTALLED_APPS = [
      ....
      'corsheaders'
  ]
  
  MIDDLEWARE = [
      ....
      'corsheaders.middleware.CorsMiddleware'
  ]
```

- 在settings开启允许跨域：

```
CORS_ORIGIN_ALLOW_ALL = True
```

  

## Vue配置[ElementUI](https://element.eleme.cn/)

- 安装插件（在项目目录下）

```
cnpm install element-ui
```

- 在main.js中配置：

```js
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI);
```

- 使用：看[官方文档](https://element.eleme.cn/#/zh-CN/component/layout)  复制粘贴



## Vue配置jQuery + bootstrap

- 先安装jQuery

```
cnpm install jquery
```

- 在vue项目中配置jQuery，在`vue.config.js`文件中配置

```js
  const webpack = require("webpack");
  
  module.exports = {
      configureWebpack: {
          plugins: [
              new webpack.ProvidePlugin({
                  $: "jquery",
                  jQuery: "jquery",
                  "window.jQuery": "jquery",
                  Popper: ["popper.js", "default"]
              })
          ]
   		}
  };
```

- 安装bootstrap插件

```
cnpm install bootstrap@3
```

- 在vue项目中配置 bootstrap，在main.js 中配置

```js
import "bootstrap"
import "bootstrap/dist/css/bootstrap.css"
```

  

