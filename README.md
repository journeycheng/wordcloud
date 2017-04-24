# wordcloud

## 一、基于固定词条的方式
### 已经有了切分好的信息段，比如特征值。在画词图之前，需要做好统计工作
```python
import json
def wordCount(filename):
    word = {}
    file = open(filename, 'r')
    while True:
        line = file.readline()
        if line:
            line = json.loads(line)
            if 'zone_cn' in line:
                word[line['zone_cn']] = word.get(line['zone_cn'], 0) + 1
        else:
            break
    return word
```
- 根据自己的需求构造字典，上面的代码只是一个例子，用的是以前爬下来的数据。
- 返回一个字典{'key':count}就行，不需要转换成列表，不需要转换成列表，不需要转换成列表

### 调用wordcloud
```python
from scipy.misc import imread
from wordcloud import WordCloud
import matplotlib.pyplot as plt

def generatedCloud(filename, imagename, cloudname, fontname):
    coloring = imread(imagename)
    wc = WordCloud(background_color='white', mask=coloring, font_path=fontname, max_words=100, random_state=42)
    wc.fit_words(wordCount(filename))
    wc.to_file(cloudname)
    
generatedCloud(filename='chengjiaoupdate_data.json', imagename='circle.jpg', cloudname='cloud.png', fontname='skt.ttf')
```
- 调用WordCloud.fit_words函数
- skt.ttf是楷体文件
- 效果

![](./image/circle.jpg?raw=true) ![](./image/cloud.png?raw=true)

## 二、基于jieba切分词条的方式

### 唐诗切分
- 唐诗来源于：https://github.com/jackeyGao/chinese-poetry
```python
import os
import json

def getfiles(songPath):
    fileList = []
    if os.path.isdir(songPath):
        fileNames = os.listdir(songPath)
        if len(fileNames) > 0:
            for fn in fileNames:
                if os.path.isfile(os.path.join(songPath, fn)):
                    fullFN = os.path.join(songPath, fn)
                    fileList.append(fullFN)
    return fileList

def getContext(files):
    poet = ''
    for filePath in files:
        fileTxt = open(filePath, 'r')
        try:
            allText = fileTxt.read()
            allText = json.loads(allText)
            for each in allText:
                if 'paragraphs' in each:
                    for item in each['paragraphs']:
                        poet += item
        finally: fileTxt.close()
    return poet
```
- 主要思路是将唐诗合并成一个字符串

```
from scipy.misc import imread
from wordcloud import WordCloud
import matplotlib.pyplot as plt
import jieba

def draw_wordcloud(filePath, imagename, cloudname, fontname):
    poet = getContext(getfiles(filePath))
    cut_text = ' '.join(jieba.cut(poet))
    coloring = imread(imagename)
    wc = WordCloud(background_color='white', mask=coloring, font_path=fontname, max_words=1000, prefer_horizontal=1)
    wc.generate(cut_text)
    wc.to_file(cloudname)

draw_wordcloud(filePath='./json/tang', imagename='tang_back.png', cloudname="tang.png", fontname='ckt.ttf')
```
- 设置frefer_horizontal=1是为了让所有的词条水平显示
- 效果：
![](./image/tang_back.png?raw=true) ![](./image/tang_poem.png?raw=true)










