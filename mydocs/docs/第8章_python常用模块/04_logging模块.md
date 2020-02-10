## logging模块

v1:

```python
import logging

# 日志级别（如果不设置，默认显示30以上）
logging.info('info')   # 10
logging.debug('debug')     # 20
logging.warning('warning')   # 30
logging.error('error')     # 40
logging.critical('critical')  # 50

# 打印结果：
'''
WARNING:root:warning
ERROR:root:error
CRITICAL:root:critical
'''
```

v2:

```python
import logging

# 日志的基本配置

logging.basicConfig(filename='access.log',
                    format='%(asctime)s - %(name)s - %(levelname)s -%(module)s: %(message)s',
                    datefmt='%Y-%m-%d %H:%M:%S %p',
                    level=10)  # level是等级，10以上的都记录日志

logging.info('正常信息')  # 10
logging.debug('调试信息')  # 20
logging.warning('警告信息')  # 30
logging.error('报错信息')  # 40
logging.critical('严重错误信息')  # 50


# 会创建一个access.log日志：
'''
2019-09-27 21:57:45 PM - root - DEBUG -test: 调试信息
2019-09-27 21:57:45 PM - root - INFO -test: 正常信息
2019-09-27 21:57:45 PM - root - WARNING -test: 警告信息
2019-09-27 21:57:45 PM - root - ERROR -test: 报错信息
2019-09-27 21:57:45 PM - root - CRITICAL -test: 严重错误信息

'''
```

v3: 自定义配置

```python
import logging

# 1. 配置logger对象
cwz_logger = logging.Logger('cwz')
neo_logger = logging.Logger('neo')

# 2. 配置格式
formmater1 = logging.Formatter('%(asctime)s - %(name)s -%(thread)d - %(levelname)s -%(module)s:  %(message)s',
                               datefmt='%Y-%m-%d %H:%M:%S %p ', )

formmater2 = logging.Formatter('%(asctime)s :  %(message)s',
                               datefmt='%Y-%m-%d %H:%M:%S %p', )

formmater3 = logging.Formatter('%(name)s %(message)s', )

# 3. 配置handler --> 往文件打印or往终端打印
h1 = logging.FileHandler('cwz.log')
h2 = logging.FileHandler('neo.log')
sm = logging.StreamHandler()

# 4. 给handler配置格式
h1.setFormatter(formmater1)
h2.setFormatter(formmater2)
sm.setFormatter(formmater3)

# 5. 把handler绑定给logger对象
cwz_logger.addHandler(h1)
cwz_logger.addHandler(sm)
neo_logger.addHandler(h2)

# 6. 直接使用
cwz_logger.info(f'cwz 购买 变形金刚 8个')
```