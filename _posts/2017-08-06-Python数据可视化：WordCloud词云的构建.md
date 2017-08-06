---
title: Python数据可视化：饼状图
categories:
 - Python数据可视化
---



在Python中构建词云，采用的是wordcloud库
官网: https://amueller.github.io/word_cloud/ 
github: https://github.com/amueller/word_cloud 

下面对词云的构建进行代码实现：

```
# -*- coding:utf-8 -*-
from os import path
from scipy.misc import imread
import matplotlib.pyplot as plt
import jieba

from wordcloud import WordCloud, STOPWORDS, ImageColorGenerator

stopwords = {}


# 加载stopwords
def importStopword(filename=''):
    global stopwords
    f = open(filename, 'r')
    line = f.readline().rstrip()

    while line:
        stopwords.setdefault(line, 0)
        stopwords[line] = 1
        line = f.readline().rstrip()

    f.close()


# 中文分词（jieba）
def processChinese(text):
    seg_generator = jieba.cut(text)  # 使用结巴分词，也可以不使用
    seg_list = [i for i in seg_generator if i not in stopwords]
    seg_list = [i for i in seg_list if i != u' ']
    seg_list = r' '.join(seg_list)

    return seg_list


importStopword(filename='./stopwords.txt')

# 获取当前文件路径
# __file__ 为当前文件, 在ide中运行此行会报错,可改为
# d = path.dirname('.')
d = path.dirname(__file__)

text = open(path.join(d, 'love.txt')).read()  # 读取love.txt文件内容

# 如果是中文，使用结巴分词
text = processChinese(text)

# 设置背景图片
back_coloring = imread(path.join(d, "./image/love.jpg"))

# 生成词云, 可以用generate输入全部文本(中文不好分词),也可以我们计算好词频后使用generate_from_frequencies函数
wc = WordCloud(font_path='./font/lll.ttf',  # 设置字体
               background_color="black",  # 背景颜色
               max_words=2000,  # 词云显示的最大词数
               mask=back_coloring,  # 设置背景图片
               # max_font_size=100, #字体最大值
               random_state=42,
               ).generate(text)

# 从背景图片生成颜色值
image_colors = ImageColorGenerator(back_coloring)

# 绘制词云
plt.figure()
plt.imshow(wc.recolor(color_func=image_colors))
plt.axis("off")
plt.show()

# 保存图片
wc.to_file(path.join(d, "love.png"))

```

词云显示：
![这里写图片描述](http://img.blog.csdn.net/20161108210814324)