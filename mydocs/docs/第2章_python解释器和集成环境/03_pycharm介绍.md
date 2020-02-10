# pycharm简介

PyCharm是jetbrains 开发的 一款功能强大的 Python 编辑器，具有跨平台性，下载地址：[官网](https://www.jetbrains.com/pycharm/download/)， 可以选择不同平台下载，社区版是免费的，专业版收费，功能也多一些。虽然也有专业版破解的，但是还是要支持正版。

# pycharm安装

windows平台 安装没啥好说的，一路next就行了。

Linux平台安装：

```bash
# 下载tar.gz包

# 解压
tar -xzvf pycharm-professional-2018.3.2.tar.gz

# 将解压的包移动到你存放软件的目录，linux下一般软件存放在/opt 下
mv pycharm-2019.3.2 /opt/

# 为pycharm创建快捷方式
sudo vim /usr/share/applications/pycharm.desktop

# 写入如下内容
[Desktop Entry]
Type=Application
Name=Pycharm
GenericName=pycharm-2019.3
Comment=Pycharm3:The Python IDE
Exec=sh /opt/pycharm-2019.3.2/bin/pycharm.sh        # 执行文件
Icon=/opt/pycharm-2019.3.2/bin/pycharm.png         # 图标
Terminal=pycharm
Categories=Pycharm;
```

破解的话，请参考：https://zhile.io/2018/08/25/jetbrains-license-server-crack.html



# pycharm快捷键

pycharm快捷键有很多，常用的如下：

```
ctrl + c  复制，  默认复制整行
ctrl + v 粘贴
ctrl + x  剪切
ctrl + a   全选
ctrl + z  撤销
ctrl + f  查找
ctrl + shift + z   反撤销

ctrl + d  复制粘贴选中内容，没有选中默认整行
ctrl + y  删除整行
ctrl + backspace   删除一个单词
ctrl + w  选中一个单词
ctrl + shift + r  全局搜索
shift + F10   运行上一个文件
ctrl + shift + F10  运行当前文件
shift + enter  进入下一行
ctrl + /   整体注释
ctrl + alt + l  格式化代码
home  回到行首
end  回到行尾
```

