# 修改python的pip源为国内源



由于网络原因，访问国外的pip源很慢，因此可将源改为国内源（都是pipy官网的镜像），就能流畅的下载了。

## 临时修改

临时修改：在后边加个-i参数指定pip源，如下所示：

```python
pip install package_name -i https://pypi.tuna.tsinghua.edu.cn/simple
```



## 永久修改

**windows**：

- 进入%APPDATA%目录
- 新建pip文件夹
- 在pip文件夹内新建pip.ini文件，设置pip源，如下设置清华大学的pip源

```bash

[global]
index-url=https://pypi.tuna.tsinghua.edu.cn/simple
timeout = 6000
 
[install]
trusted-host=pypi.tuna.tsinghua.edu.cn
 
disable-pip-version-check = true
```

**linux**:

```bash
mkdir ~/.pip
vim ~/.pip/pip.conf

# 添加源
[global]
index-url=https://pypi.tuna.tsinghua.edu.cn/simple
timeout = 6000
 
[install]
trusted-host=pypi.tuna.tsinghua.edu.cn
```

其他国内源：

```
豆瓣   http://pypi.douban.com/
 
华中理工大学   http://pypi.hustunique.com/ 
 
山东理工大学   http://pypi.sdutlinux.org/ 
 
中国科学技术大学   http://pypi.mirrors.ustc.edu.cn/ 
 
阿里云   http://mirrors.aliyun.com/pypi/simple/ 
```

