# wordcloud

## 基于固定词条的方式
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
- wordcloud.jpg
[](./image/circle.jpg)
