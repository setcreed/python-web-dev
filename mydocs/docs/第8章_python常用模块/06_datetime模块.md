# datetime模块

```python
import datetime

# 输出当前时间
print(datetime.datetime.now())
# 2019-09-28 17:25:24.551237


# 加时间
now = datetime.datetime.now()
print(now + datetime.timedelta(days=3))
# 2019-10-01 17:28:24.710093


# 时间替换
print(now.replace(year=1940))
# 1940-09-28 17:29:45.066855
```

