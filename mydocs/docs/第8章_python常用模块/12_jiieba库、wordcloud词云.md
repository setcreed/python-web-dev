# jieba库

jieba库一般用于分词

```python
import jieba

res = jieba.lcut('中华人民共和国是一个伟大的国家')   # 精确模式，返回一个列表类型的分词结果
print(res)

# 打印结果：
['中华人民共和国', '是', '一个', '伟大', '的', '国家']
import jieba

res = jieba.lcut_for_search('中华人民共和国是一个伟大的国家')  # 搜索引擎模式，返回一个列表类型的分词结果，存在冗余
print(res)

# 打印结果：
['中华', '华人', '人民', '共和', '共和国', '中华人民共和国', '是', '一个', '伟大', '的', '国家']
import jieba

res = jieba.lcut('中华人民共和国是一个伟大的国家',cut_all=True) # 把所有的可能全部切出来
print(res)

# 打印结果：
['中华', '中华人民', '中华人民共和国', '华人', '人民', '人民共和国', '共和', '共和国', '国是', '一个', '伟大', '的', '国家']
```

# wordcloud词云

```python
import wordcloud
import jieba
from imageio import imread

mk = imread('test.png')  # 把图片读入内存
s = '''当其他人盲目的追寻真相和真实的时候，记住。万物皆虚。
当其他人受到法律和道德的束缚的时候，记住。万事皆允。
我们服侍光明却耕耘于黑暗。
真正睿智的人不会向你指明真相，而是教导你去发现真相。
世界上明明有一万种宗教，人们却用一种方式祈祷。这里没有上帝，只有属于我们自己的信条。
我们在黑暗中工作，为光明服务，我们，是刺客。'''

s_list = jieba.lcut(s)  # 把字符串切成列表
s = ' '.join(s_list)  # 把列表拼接成字符串
w = wordcloud.WordCloud(font_path='C:\Windows\Fonts\simkai.ttf', background_color='white', mask=mk)
w.generate(s)
w.to_file('set.png')
```



![](https://cdn.jsdelivr.net/gh/setcreed/pic_img/cdn_img/20200131205946.png)