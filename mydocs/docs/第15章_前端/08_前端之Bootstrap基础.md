# bootstrap介绍

详情见：[bootstrap](https://v3.bootcss.com/)

# 布局容器

使用前端框架，所有标签样式的调整，全都是通过class属性值来的

```html
<div class="container"></div>        左右两边留白

<div class="container-fluid"></div>  全屏显示
```

# 格栅系统

```html
<div class="row c1"></div>
```

一个row就是一行，每个row默认会分成12份，通过`class="col-md-num"`来选择你想要占几份

根据浏览器窗口的不同，采取不同的布局方式，进行响应式布局，

`col-md-num`进行PC端布局，`col-sm-num`进行移动端布局

```html
@media screen and (max-width: 700px){
            .c1 {
            height: 50px;
            background-color: yellow;
            border: 3px solid blue;
        }

当浏览器窗口宽度为700px时，颜色会变为黄色，这也是响应式布局
```

[栅格系统参数](<https://v3.bootcss.com/css/#grid-options>)      `xs   sm  md  lg`    

# 表格

常见参数：

```html
<table class="table table-striped table-bordered table-hover">
```

`table-striped`  条纹状表格

`table-bordered`  带边框的表格

`table-hover`      鼠标悬停

# 表单

表单加样式，记住一个参数：`form-control`

# 按钮组

btn btn-颜色



# 小练习：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    <link rel="stylesheet" href="font-awesome-4.7.0/font-awesome.min.css">
    <link href="https://cdn.bootcss.com/twitter-bootstrap/3.4.1/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdn.bootcss.com/twitter-bootstrap/3.4.1/js/bootstrap.min.js"></script>
</head>
<body>
<!--导航条开始-->
<nav class="navbar navbar-inverse">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                    data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">图书管理系统</a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
                <li class="active"><a href="#">图书<span class="sr-only">(current)</span></a></li>
                <li><a href="#">作者</a></li>
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
                       aria-expanded="false">更多 <span class="caret"></span></a>
                    <ul class="dropdown-menu">
                        <li><a href="#">Action</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">Separated link</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">One more separated link</a></li>
                    </ul>
                </li>
            </ul>
            <form class="navbar-form navbar-left">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="关键字">
                </div>
                <button type="submit" class="btn btn-default">Submit</button>
            </form>
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">Assassin</a></li>
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
                       aria-expanded="false">更多 <span class="caret"></span></a>
                    <ul class="dropdown-menu">
                        <li><a href="#">Action</a></li>
                        <li><a href="#">Another action</a></li>
                        <li><a href="#">Something else here</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">Separated link</a></li>
                    </ul>
                </li>
            </ul>
        </div><!-- /.navbar-collapse -->
    </div><!-- /.container-fluid -->
</nav>
<!--导航条结束-->

<div class="container">
    <div class="row">
        <div class="col-md-3">
            <div class="list-group">
                <a href="#" class="list-group-item active">
                    首页
                </a>
                <a href="#" class="list-group-item">图书列表</a>
                <a href="#" class="list-group-item">作者列表</a>
                <a href="#" class="list-group-item">出版社</a>
                <a href="#" class="list-group-item">更多</a>
            </div>
        </div>
        <div class="col-md-9">
            <div class="panel panel-primary">
                <div class="panel-heading">图书管理系统 <span class="glyphicon glyphicon-book pull-right"></span></div>
                <div class="panel-body">
                    <form class="form-inline">
                        <div class="form-group">
                            <label class="sr-only" for="exampleInputAmount">Amount (in dollars)</label>
                            <div class="input-group">
                                <input type="text" class="form-control" id="exampleInputAmount" placeholder="关键字">
                            </div>
                        </div>
                        <button type="submit" class="btn btn-primary">搜素</button>
                        <a href="" class="btn btn-primary pull-right">新增</a>
                    </form>

                    <br>
                    <br>
                    <table class="table table-hover table-bordered table-striped">
                        <thead>
                        <tr class="success">
                            <th>序号</th>
                            <th>书名</th>
                            <th>作者</th>
                            <th>价格</th>
                            <th>出版社</th>
                            <th>操作</th>
                        </tr>
                        </thead>
                        <tbody>
                        <tr>
                            <td>1</td>
                            <td>人间失格</td>
                            <td>太宰治</td>
                            <td>89.98</td>
                            <td>南方出版社</td>
                            <td>
                                <a href="" class="btn btn-primary btn-sm">编辑</a>
                                <a href="" class="btn btn-danger btn-sm">删除</a>
                            </td>
                        </tr>
                        </tbody>
                        <tbody>
                        <tr>
                            <td>1</td>
                            <td>人间失格</td>
                            <td>太宰治</td>
                            <td>89.98</td>
                            <td>南方出版社</td>
                            <td>
                                <a href="" class="btn btn-primary btn-sm">编辑</a>
                                <a href="" class="btn btn-danger btn-sm">删除</a>
                            </td>
                        </tr>
                        </tbody>
                        <tbody>
                        <tr>
                            <td>1</td>
                            <td>人间失格</td>
                            <td>太宰治</td>
                            <td>89.98</td>
                            <td>南方出版社</td>
                            <td>
                                <a href="" class="btn btn-primary btn-sm">编辑</a>
                                <a href="" class="btn btn-danger btn-sm">删除</a>
                            </td>
                        </tr>
                        </tbody>
                        <tbody>
                        <tr>
                            <td>1</td>
                            <td>人间失格</td>
                            <td>太宰治</td>
                            <td>89.98</td>
                            <td>南方出版社</td>
                            <td>
                                <a href="" class="btn btn-primary btn-sm">编辑</a>
                                <a href="" class="btn btn-danger btn-sm">删除</a>
                            </td>
                        </tr>
                        </tbody>
                        <tbody>
                        <tr>
                            <td>1</td>
                            <td>人间失格</td>
                            <td>太宰治</td>
                            <td>89.98</td>
                            <td>南方出版社</td>
                            <td>
                                <a href="" class="btn btn-primary btn-sm">编辑</a>
                                <a href="" class="btn btn-danger btn-sm">删除</a>
                            </td>
                        </tr>
                        </tbody>
                        <tbody>
                        <tr>
                            <td>1</td>
                            <td>人间失格</td>
                            <td>太宰治</td>
                            <td>89.98</td>
                            <td>南方出版社</td>
                            <td>
                                <a href="" class="btn btn-primary btn-sm">编辑</a>
                                <a href="" class="btn btn-danger btn-sm">删除</a>
                            </td>
                        </tr>
                        </tbody>
                        <tbody>
                        <tr>
                            <td>1</td>
                            <td>人间失格</td>
                            <td>太宰治</td>
                            <td>89.98</td>
                            <td>南方出版社</td>
                            <td>
                                <a href="" class="btn btn-primary btn-sm">编辑</a>
                                <a href="" class="btn btn-danger btn-sm">删除</a>
                            </td>
                        </tr>
                        </tbody>
                        <tbody>
                        <tr>
                            <td>1</td>
                            <td>人间失格</td>
                            <td>太宰治</td>
                            <td>89.98</td>
                            <td>南方出版社</td>
                            <td>
                                <a href="" class="btn btn-primary btn-sm">编辑</a>
                                <a href="" class="btn btn-danger btn-sm">删除</a>
                            </td>
                        </tr>
                        </tbody>


                    </table>
                </div>
            </div>
        </div>
    </div>
</div>

</body>
</html>
```

![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200209104200.png)