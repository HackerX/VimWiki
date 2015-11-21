%toc
----
# 编程之道
师曰：『三日不编程，食肉无味。』——《编程之道·先贤篇·其一》

## 选择

| 分类        | 名称         | 平台  | 描述                          |
|-------------|--------------|-------|-------------------------------|
| Linux发行版 | Arch Linux   | Linux |                               |
| 编程语言    | Python       | All   |                               |
| 编辑器      | VIM          | All   |                               |
| 文件管理器  | Ranger       | Linux | VIM风格的文件管理器           |
| PDF阅读器   | MuPDF        | All   | VIM风格的PDF阅读器            |
| 浏览器      | Firefox      | All   |                               |
| 播放器      | Mplayer、VLC | All   |                               |

# Python
### 多维列表指定列排序

```python
def mySort2(a, indexLie=0):
    nLie = len(a[0])
    if indexLie < 0:
       indexLie = 0
    elif indexLie >= nLie:
        indexLie = 0
    if indexLiei != 0:
        for i in range(len(a)):
            (a[i][0], a[i][indexLie]) = (a[i][indexLie], a[i][0])
    a.sort()
    if indexLie != 0:
        for i in range( len(a)):
            (a[i][0], a[i][indexLie]) = (a[i][indexLie], a[i][0])

X = [[100,1,2,5,"李四" ],
    [-1 ,1.2,2.1,5,"张三" ],
    [88 ,9.1,"王五" ],
    [ 9 ,0.5,3,"杨四"]]

mySort2(X, 1)
```

一样的功能，不过是新复制了一份列表

	sorted(alist, key=lambda a: a[2])

### 列表推导式

### 字符串3函数
* strip()函数: 剥皮字符串
* join()函数: 连接字符串
* split()函数: 切割字符串

## 解决编码问题
* encode('utf-8') #编码
* decode('utf-8') #解码
* unicode() #QTString转换成默认unicode
    
    UnicodeEncodeError: 'ascii' codec can't encode character u'\u8888' in position 0: ordinal not in range(168)
    ascii码无法被转换成unicode码，python2.7默认编码是ascii
    import sys
    print(sys.getdefaultencoding())
    当目标文件为utf-8，或读取的文件为utf-8时，系统就常识以ascii格式处理，所以就报错。
    解决方法：

    方法一：在python代码中进行改变

    import sys  
    reload(sys)  
    sys.setdefaultencoding('utf-8') 
    注意：使用此方式，有极大的可能导致print函数无法打印数据！

    方法二：python安装目录下的lib\site-packages文件夹下新建一个sitecustomize.py，文件中的代码为：
    import sys  
    sys.setdefaultencoding('utf-8')   
    这是比较推荐的方式。

## Python库

| 名称          | 功能     | 评分 | 链接 |
|---------------|----------|------|------|
| Psyco         | 加速     | 5    |      |
| PyQt(PySide)  | QT界面   | 5    |      |
| PIL           | 图像处理 | 5    |      |
| Requests      | 访问Web  | 5    |      |
| BeautifulSoop | 解析Html | 5    |      |

## Regex
```python
regex = re.search(ur'''(.*(商品|货物)).*\W(\w*)(-淘宝网|-tmall.com天猫)$''', content)
#title = regex.group(3)
title = regex.group(1) + ' ' + regex.group(3)
print title
```

## Web编程

### Requests
```python
import requests
#r = requests.get(url, headers=self.headers, proxies=self.proxies)#直接访问

s = requests.Session()#创建会话
r = s.get(url, headers=headers)#获取页面
#r.encoding = 'gb18030'#设置编码（中文乱码解决方案）
r.encoding =  r.apparent_encoding #自动编码设定
r = s.post(url, headers=headers, data=playload)#post请求页面
r = s.get(url, headers=headers)#获取页面

r.text#字节形式
r.content#二进制形式

proxies = {'http':'http://127.0.0.1:8087', 'https':'http://127.0.0.1:8087'}
proxies = {'http':'http://user:pass@127.0.0.1:8087', }#代理若需HTTP Basic Auth验证

r = s.get(url, headers=headers, proxies=proxies)
```

### BeautifulSoop
```python
from bs4 import BeautifulSoup  
soup = BeautifulSoop(r.text)
for i in soup.find_all('a')
    print i

soup.title.string
```

* 查找class标记

```python
find(attrs={"class":"content"})
soup.find_all("a", attrs={"class":"content"})
find('div', 'content')
soup.find_all('dt', 'title')#class属性无需写属性名.
```

* 通过文本进行查找

```python
soup.find_all(text="Elsie")
soup.find_all(text=["Tillie", "Elsie", "Lacie"])
```

* 限制结果个数

```python
soup.find_all("a", limit=2)
```

* 可以这样用，定点打击

```python
soup.find('div',id='doc').find('div',id='scontainer').find.... 
```

* 获取标记内参数

```python
a.get('href')
```

==== 登陆分析 ====

[如何分析模拟登陆网站的内部逻辑过程（crifan.com）](http://www.crifan.com/use_ie9_f12_to_analysis_the_internal_logical_process_of_login_baidu_main_page_website)

=== 线程 ===
* 最简单的线程

```python
import thread
thread.start_new_thread(self.compare, ())
```

不推荐，必须设置seelp时间，否则线程提前退出。且难以获取返回值！

* threading

```python
import threading

def worker():
    print 'worker'
    time.sleep(1)
    return

for i in xrange(5):
    t = threading.Thread(target=worker)
    t.start()
```

* threading类封装

```python
import threading, time
class Worker(threading.Thread):
    def __init__(self, num, interval):
        threading.Thread.__init__(self)
        self.thread_num = num
        self.interval = interval
        self.thread_stop = False

    def run(self):
        while not self.thread_stop:
            print 'Thread Object(%d), Time:%s\n' %(self.thread_num, time.ctime())
            time.sleep(self.interval)

    def stop(self):
        self.thread_stop = True

    thread1 = Worker(1, 1)
    thread2 = Worker(2, 2)
    thread1.start()
    thread2.start()
    time.sleep(10)
    thread1.stop()
    thread2.stop()
```

== 单片机与传感器 ==
=== 温度传感器 ===
* DS18B20数字温度传感器
美国达拉斯（Dallas）半导体公司制造，输出数字信号，支持一线总线，测量温度范围为-55°C至+125℃，误差±0.5℃。

* PT100铂电阻温度传感器
精度高，稳定性好，应用温度范围广，测量温度范围为-200℃至400℃。

* LM35模拟温度传感器
输出模拟信号，0度输出电压0v，温度每升高1度，输出电压增加10mv，测量温度范围为0℃至100℃，误差±0.5℃。

## VIM
### 常用插件

| 名称    | 功能         | 评分 | 链接 |
|---------|--------------|------|------|
| VIMWIKI | 个人知识管理 | 5    |      |

### 其它问题

* U盘介质受写入保护

    CMD >> chkdsk H: /F
    
### excel vlookup函数

vlookup函数是excel中的高阶用法

[例子] 这个例子检索J6单元格是否存在于B6-D999单元格中

	=VLOOKUP(J6,$B$6:$D$D999,3,FALSE)

[例子] 这个例子检索A2单元格是否存在于C2-C999单元格中

	=VLOOKUP(A2,$C$2:$C$999,1,FALSE)

----

=== 简书 ===

诸行无常，是生灭法，生灭灭已，寂灭为乐。——《涅磐经》

《涅磐经》中佛教的无常观。其大意是指，世间的一切无时不在生住异灭中，过去有的，现在起了变异；现在有的，将来终归幻灭。

花朵艳丽终散落，谁人世间能长久，今日攀越高山岭，醉生梦死不再有。——《伊吕波歌》

极富诗意的日语《伊吕波歌》，就是宣扬佛教的万物无常的教义。

人生天地之间，若白驹过隙，忽然而已。——《庄子•知北游》

线粒体功能的衰落被发现与蛋白质SIRT1的缺乏有关。

万物无常，洒脱的活着、执着的追求，生死由生死。
