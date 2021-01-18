# 

# 一、爬虫基础简介

## 1.爬虫相关概念

> - 爬虫的概念：通过编写程序，模拟浏览器上网，然后让其去互联网上抓取数据的过程
> - 爬虫的合法性：
>   - 在法律中是不被禁止的
>   - 具有违法风险：干扰了被访问网站的正常运营；抓取了受到法律保护的特定类型的数据或信息
> - 爬虫的分类
>   - 通用爬虫：抓取系统重要组成部分，抓取的是一整张页面数据
>   - 聚焦爬虫：建立在通用爬虫基础之上，抓取的是页面中特定的局部内容
>   - 增量式爬虫：检测网站中数据更新的情况，只会抓取网站中最新更新出来的数据
> - 反爬机制：门户网站可以通过制定相应的策略或者技术手段，防止爬虫程序进行网站数据的爬取
> - 反反爬策略：爬虫程序可以通过指定相关的策略或者技术手段，破解门户网站中具备的反爬机制，从而可以获取门户网站中相关的数据
> - robots.txt 协议(君子协议)：规定了网站中哪些数据可以被爬虫爬取，哪些数据不可以被爬取

## 2. http & https 协议

> - http 协议：就是服务器和客户端进行数据交互的一种形式
>   - 常用请求头信息：
>     - User-Agent：请求载体的身份标识
>     - Connection：请求完毕后，是断开连接还是保持连接
>   - 常用响应头信息
>     - Content-Type：服务器响应回客户端的数据类型
> - https 协议：安全的超文本传输协议
>   - 加密方式
>     - 对称秘钥加密
>     - 非对称秘钥加密
>     - 整数秘钥加密

# 二、requests 模块

> - python 中原生的一款基于网络请求的模块，功能强大，简单便捷，效率极高
> - 作用：模拟浏览器发请求
> - requests 模块的使用(编码流程)
>   1. 指定 url
>   2. 发起请求
>   3. 获取相应数据
>   4. 持久化存储

- 爬取搜狗首页信息

```python
import requests
# 1. 指定url
url = 'https://www.sogou.com/'
# 2. 发起请求
response = requests.get(url=url)
# 3. 获取相应数据
#   text--返回字符串形式的相应数据
page_text = response.text
# 4. 持久化存储
fp = open('./sogou.html', 'w', encoding='utf8')
fp.write(page_text)
fp.close()
print('over!')
```

> ![](https://gitee.com/yao_zhimin/myimg/raw/master/20210114200937.png)

> UA(User-Agent)检测：门户网站的服务器会检测对应请求的载体身份标识，如果检测到请求的载体身份标识为某一款浏览器，说明该请求是一个正常的请求。如果检测到请求载体的身份标识不是某一款浏览器，则表示该请求是不正常的请求(爬虫)，服务器端很可能拒绝该请求
>
> UA 伪装：让爬虫对应的请求载体身份标识伪装成某一款浏览器

- 网页采集器

```python
import requests
# UA伪装
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36 Edg/87.0.664.75'
}
# 1. 指定url
# 将url携带的参数封装到字典中
kw = input('enter a word:')
param = {
    'query': kw
}
url = 'https://www.sogou.com/web'
# 2. 发起请求
# 对指定的url发起的请求对应的url是携带参数的，并且请求过程中处理了参数
response = requests.get(url, params=param, headers=headers)
# 3. 获取相应数据
page_text = response.text
# 4. 持久化存储
filename = kw+'.html'
fp = open(filename, 'w', encoding='utf8')
fp.write(page_text)
fp.close()
print('over!')
```

> ![](https://gitee.com/yao_zhimin/myimg/raw/master/20210114203341.png)

- 破解百度翻译

```python
#post请求(携带了参数)
#响应数据是一组json数据

import requests
import json
# 1. 指定url
post_url = 'https://fanyi.baidu.com/sug'
# 2.UA伪装
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36 Edg/87.0.664.75'
}
# 3.post请求参数处理(同get请求一致)
word = input('enter a word:')
data = {
    'kw': word
}
# 4.请求发送
response = requests.post(url=post_url, data=data, headers=headers)
# 5. 获取相应数据
#   json方法返回一个obj对象(如果确认相应数据是json类型的，才可以使用json())
dic_obj = response.json()
# print(dic_obj)
# 6. 持久化存储
filename = word+'.json'
fp = open(filename, 'w', encoding='utf8')
json.dump(dic_obj, fp=fp, ensure_ascii=False)
print('over!!')


# 输入code后运行结果如下：
"""
{"errno": 0, "data": [{"k": "code", "v": "n. 法典; 行为准则; 代码，密码; 信号 vt. 将…译成电码; 编码，加密 vi. 为…编码;"}, {"k": "Code", "v": "[人名] [英格兰人姓氏] 科德 Coad的变体"}, {"k": "CODE", "v": "abbr. client/server open development environment 客"}, {"k": "codec", "v": "编解码器"}, {"k": "Codec", "v": "abbr. compress(or) /decompress(or) 压缩/解压缩（器）"}]}
"""
```

- 爬取豆瓣电影分类排行榜

```python
import requests
import json
url = 'https://movie.douban.com/j/chart/top_list'
param = {
    'type': '13',  # 电影类型
    'interval_id': '100:90',
    'action': '',
    'start': '0',  # 从库中第n部电影开始取
    'limit': '20',  # 一次取m部
}
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36 Edg/87.0.664.75'
}
response = requests.get(url=url, params=param, headers=headers)
list_data = response.json()
fp = open('./douban.json', 'w', encoding='utf8')
json.dump(list_data, fp=fp, ensure_ascii=False)
print('over!!')
```

> ![](https://gitee.com/yao_zhimin/myimg/raw/master/20210114211606.png)

- 爬取肯德基餐厅查询地点

```python
import requests
# 1. 指定url
post_url = 'http://www.kfc.com.cn/kfccda/ashx/GetStoreList.ashx?op=keyword'
# 2. 发起请求
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36 Edg/87.0.664.75'
}
data = {
    'cname': '',
    'pid': '',
    'keyword': '北京',
    'pageIndex': '1',
    'pageSize': '10',
}
response = requests.post(url=post_url, data=data)
# 3. 获取相应数据
dic_obj = response.text
# 4. 持久化存储
fp = open('./kfc.txt', 'w', encoding='utf8')
fp.write(dic_obj)
fp.close()
print('over!!')

#运行结果为：
"""
{"Table":[{"rowcount":65}],"Table1":[{"rownum":1,"storeName":"育慧里","addressDetail":"小营东路3号北京凯基伦购物中心一层西侧","pro":"24小时,Wi-Fi,店内参观,礼品卡","provinceName":"北京市","cityName":"北京市"},{"rownum":2,"storeName":"京通新城","addressDetail":"朝阳路杨闸环岛西北京通苑30号楼一层南侧","pro":"Wi-Fi,店内参观,礼品卡,生日餐会","provinceName":"北京市","cityName":"北京市"},{"rownum":3,"storeName":"黄寺大街","addressDetail":"黄寺大街15号北京城乡黄寺商厦","pro":"Wi-Fi,点唱机,店内参观,礼品卡,生日餐会","provinceName":"北京市","cityName":"北京市"},{"rownum":4,"storeName":"四季青桥","addressDetail":"西四环北路117号北京欧尚超市F1、B1","pro":"Wi-Fi,礼品卡,生日餐会","provinceName":"北京市","cityName":"北京市"},{"rownum":5,"storeName":"亦庄","addressDetail":"北京经济开发区西环北路18号F1＋F2","pro":"24小时,Wi-Fi,礼品卡,生日餐会","provinceName":"北京市","cityName":"北京市"},{"rownum":6,"storeName":"石园南大街","addressDetail":"通顺路石园西区南侧北京顺义西单商场石园分店一层、二层部分","pro":"24小时,Wi-Fi,店内参观,礼品卡,生日餐会","provinceName":"北京市","cityName":"北京市"},{"rownum":7,"storeName":"北京南站","addressDetail":"北京南站候车大厅B岛201号","pro":"Wi-Fi,礼品卡","provinceName":"北京市","cityName":"北京市"},{"rownum":8,"storeName":"北清路","addressDetail":"北京北清路1号146区","pro":"24小时,Wi-Fi,店内参观,礼品卡","provinceName":"北京市","cityName":"北京市"},{"rownum":9,"storeName":"大红门新世纪肯德基餐厅","addressDetail":"海户屯北京新世纪服装商贸城一层南侧","pro":"Wi-Fi,点唱机,礼品卡","provinceName":"北京市","cityName":"北京市"},{"rownum":10,"storeName":"巴沟","addressDetail":"巴沟路2号北京华联万柳购物中心一层","pro":"Wi-Fi,礼品卡,生日餐会","provinceName":"北京市","cityName":"北京市"}]}
"""
```

- 爬取化妆品企业详情信息

```python
import requests
import json
post_url = 'http://scxk.nmpa.gov.cn:81/xk/itownet/portalAction.do?method=getXkzsList'
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36 Edg/87.0.664.75'
}
id_list = []    # 存储企业的id
all_detail_info = []    # 存储所有的企业详情数据
for page in range(1, 6):  # 获取前5页数据
    data = {
        'on': 'true',
        'page': str(page),
        'pageSize': '15',
        'productName': '',
        'conditionType': '1',
        'applyname': '',
        'applysn': '',
    }
    json_ids = requests.post(url=post_url, data=data, headers=headers).json()
    for dic in json_ids['list']:
        id_list.append(dic['ID'])
# print(id_list)
# 获取企业详情数据
in_post_url = 'http://scxk.nmpa.gov.cn:81/xk/itownet/portalAction.do?method=getXkzsById'

for id in id_list:
    in_data = {
        'id': id,
    }
    detail_info = requests.post(url=in_post_url, data=in_data, headers=headers).json()
    all_detail_info.append(detail_info)
# 持久化存储
fp = open('./hzp.json', 'w', encoding='utf8')
json.dump(all_detail_info, fp=fp, ensure_ascii=False)
print('over!!')
```

> ![](https://gitee.com/yao_zhimin/myimg/raw/master/20210114222216.png)

# 三、数据解析

> 原理：解析的局部的文本内容都会在标签之间或者标签对应的属性中进行存储
>
> 1. 进行指定标签的定位
> 2. 标签或者标签对应的属性中存储的数据值进行提取(解析)
>
> 聚焦爬虫编码流程：
>
> 1. 指定 url
> 2. 发起请求
> 3. 获取相应数据
> 4. 数据解析
> 5. 持久化存储

## 1. 正则

### 1.1 基础知识

> 正则表达式的作用：对字符串进行检索和替换

+ 正则表达式规则

```python
import re
# 1.数字和字母都表示它本身
print(re.search(r'x', 'hello xyz')) #<re.Match object; span=(6, 7), match='x'>
print(re.search(r'5', '2iu3hiu533'))    #<re.Match object; span=(7, 8), match='5'>

# 2.很多字母前面添加 \ 会有特殊含义(重难点)
#   \n --表示换行
#   \t --表示一个制表符
#   \s --表示空白字符
#   \S --表示非空白字符
#   \d --表示数字，等价于[0-9]
#   \D --表示非数字，等价于[^0-9]
#   \w --表示数字、字母及下划线以及中文等 非标点符号
#   \W --表示\w取反

# 3.绝大多数标点符号都有特殊含义
#   () --用来表示一个分组
#   如果要表示()，需要使用 \(\)
m = re.search(r'h(\d+)x', 'sh4768xflsa')
print(m.group())  # h4768x
print(m.group(1))  # 4768
m1 = re.search(r'\(.*\)', '(1+1)*3+5')
print(m1.group())  # (1+1)

#   . --表示除了换行以外的其他任意字符
#   如果想要匹配 . 需要使用 \.
m = re.search(r'm.*a', 'oaiehgmgagas')
print(m)  # <re.Match object; span=(6, 11), match='mgaga'>

#   [] --用来表示可选项范围 [x-y]表示从x到y区间，包含x和y
print(re.search(r'f[0-5]m','pdsf6m'))   # None
print(re.search(r'f[0-5a-dx]m','pdsf3m'))   #<re.Match object; span=(3, 6), match='f3m'>; (0<=val<=5 | a<=val<=d | val==x)
print(re.search(r'f[a-d]m','pdsfcm'))   #<re.Match object; span=(3, 6), match='fcm'>

#   | --用来表示或者
print(re.search(r'f(x|p|t)m','pdsfxm')) #<re.Match object; span=(3, 6), match='fxm'>
print(re.search(r'f(x|p|t)m','pdsfdm')) #None

#   {} --用来限定出现的次数
#   {n} --表示前面的元素出现n次
#   {n,} --表示前面的元素出现n次以上
#   {,n} --表示前面的元素出现n次以下
#   {m,n} --表示前面的元素出现m到n次
print(re.search(r'go{2}d','good'))  #<re.Match object; span=(0, 4), match='good'>
print(re.search(r'go{2,}d','gooood'))  #<re.Match object; span=(0, 4), match='gooood'>

#   * --表示前面的元素出现任意次数(0次及以上)，等价于{0,}
print(re.search(r'go*d','gd'))  #<re.Match object; span=(0, 2), match='gd'>
print(re.search(r'go*d','gooooooood'))  #<re.Match object; span=(0, 10), match='gooooooood'>

#   + --表示前面的元素至少出现1次，等价于{1,}
print(re.search(r'go+d','gd'))  #None
print(re.search(r'go+d','goooooood'))   #<re.Match object; span=(0, 9), match='goooooood'>

#   ? --两种用法
#       1.规定前面的元素最多只能出现一次，等价于{,1}
print(re.search(r'go?d','gd'))  #<re.Match object; span=(0, 2), match='gd'>
#       2.将贪婪模式转换成为非贪婪模式

#   ^ --以指定的内容开头,在[]里还可以表示取反； $ --以指定的内容结尾
print(re.search(r'^a.*i$','asihigshi'))   #<re.Match object; span=(0, 9), match='asihigshi'>
```

+ 贪婪模式和非贪婪模式

```python
# 在python的正则表达式里，默认是贪婪模式，尽可能多的匹配
# 默认是贪婪模式
m = re.search(r'm.*a','ohmsoahda')
print(m.group())    #msoahda
# 在贪婪模式后面添加 ? 可以将贪婪模式转换为非贪婪模式,尽可能少的匹配
m1 = re.search(r'm.*?a','ohmsoahda')
print(m1.group())    #msoa
```

+ re.Match类的使用

```python
import re
m = re.search(r'm.*a', 'oaiehgmgagas')
# 使用span方法获取匹配到的结果字符串的开始和结束下标
print(m.span())  # (6, 11)

# group方法表示正则表达式的分组
# 1.在正则表达式里使用()表示一个分组
# 2.如果没有分组，默认只有一组
# 3.分组的下标从0开始
# 可以通过分组名或者分组的下标获取到分组里匹配到的字符串
m1 = re.search(r'(2.*)(4.*)(5.*6)', 'us2ighi4esiug5aougp6ofagaufia')
# 共4组
print(m1.group(0))  # (2ighi4esiug5aougp6)第0组就是把整个正则表达式当做一个整体
print(m1.group())   # (2ighi4esiug5aougp6)默认就是拿来第0组
print(m1.group(1))  # 2ighi
print(m1.group(2))  # 4esiug
print(m1.group(3))  # 5aougp6

print(m1.groups())  # ('2ighi', '4esiug', '5aougp6')
# groupdict方法--获取到分组组成的字典
# (?P<name>表达式)  可以给分组起一个名字
m2 = re.search(r'(2.*)(?P<xxx>4.*)(5.*6)', 'us2ighi4esiug5aougp6ofagaufia')
print(m2.groupdict())  # {'xxx': '4esiug'}
print(m2.groupdict('xxx'))  # {'xxx': '4esiug'}
```

+ re.compile方法的使用

```python
# 在re模块里，可以使用re.方法调用函数，还可以调用re.compile得到一个对象
import re
m = re.search(r'm.*a', 'oaiehgmgagas')
print(m)  # <re.Match object; span=(6, 11), match='mgaga'>

r = re.compile(r'm.*a')
x = r.search('oaiehgmgagas')
print(x)  # <re.Match object; span=(6, 11), match='mgaga'>
```

+ 正则修饰符

```python
#正则修饰符是对正则表达式进行修饰

#re.S -- 让 . 匹配换行
#re.I -- 忽略大小写
#re.M -- 让 $ 匹配到换行

import re
x = re.search(r'm.*a', 'oaiehgmg\nagas', re.S)
print(x)  # <re.Match object; span=(6, 12), match='mg\naga'>
y = re.search(r'x.*', 'good Xyz', re.I)
print(y)  # <re.Match object; span=(5, 8), match='Xyz'>
z = re.findall(r'\w+$', 'i am  boy\n you are  girl\n he is  man')
print(z)  # ['man']
z1 = re.findall(r'\w+$', 'i am  boy\n you are  girl\n he is  man', re.M)
print(z1)  # ['boy', 'girl', 'man']
```

+ 正则查找相关的方法

> + 第一个参数为正则匹配规则,第二个参数为需要匹配的字符串
> + 第一种方式:在正则表达式中，如果想要匹配一个 \ 需要使用 \\\\\\\；第二种方式:在字符串前面加r，\\就表示\\\

1. match/search:查找字符串，返回的结果是一个re.Match对象

```python
# 1. 共同点：
#    + 1.只对字符串查找一次
#    + 2.返回类型都是一个re.Match对象
# 2. 不同点：
#    + match--从头开始匹配，一旦匹配失败就返回None
#    + search--在整个字符串里匹配

import re
m1 = re.match(r'hello', 'hello world good morning')
print(m1)  # <re.Match object; span=(0, 5), match='hello'>
m2 = re.match(r'good', 'hello world good morning')
print(m2)  # None
m3 = re.search(r'hello', 'hello world good morning')
print(m3)  # <re.Match object; span=(0, 5), match='hello'>
```

2. finditer:返回的结果是一个可迭代对象，可迭代对象里的数据时匹配到的所有结果，是一个re.Match对象

```python
import re
m = re.finditer(r'x', 'klxdsaighaijxixiahsxuisai')
for t in m:
    print(t)
"""
<re.Match object; span=(2, 3), match='x'>
<re.Match object; span=(12, 13), match='x'>
<re.Match object; span=(14, 15), match='x'>
<re.Match object; span=(19, 20), match='x'>
"""
```

3. findall:把查找到的所有字符串结果放到一个列表里

```python
import re
m = re.findall(r'x\d+', 'klx4dsaighaijx5ixiahsx6uisai')
print(m)  # ['x4', 'x5', 'x6']
```

4. fullmatch:完整匹配

```python
import re
m1 = re.fullmatch(r'hello', 'hello world good morning')
print(m1)  # None
m2 = re.fullmatch(r'h.*g', 'hello world good morning')
print(m2)  # <re.Match object; span=(0, 24), match='hello world good morning'>
```

+ 正则替换相关的方法

```python
#sub方法
#   第一个参数是正则表达式
#   第二个参数是替换后的新字符串或一个函数
#   第三个参数是要替换的字符串
import re
print(re.sub(r'\d','x','a9y7gay7eg8'))  #axyxgayxegx

x = 'hello34good63'
def test(x):
    y = int(x.group(0))
    y *= 2
    return str(y)
print(re.sub(r'\d+',test,x)) #hello68good126
```

### 1.2 案例

+ 爬取图片

```python
import requests

url = 'https://pic.qiushibaike.com/system/pictures/12398/123987336/medium/LS63RJAVAYHG1QJH.jpg'
# text==>字符串  content==>二进制  json()==>对象
img_data = requests.get(url).content
fp = open('./qiutu.jpg', 'wb')
fp.write(img_data)
fp.close()
print('over!!')
```

> ![](https://gitee.com/yao_zhimin/myimg/raw/master/20210117143458.png)

+ 爬取糗事百科中糗图板块下所有的糗图图片

```python
import requests
import re
import os
# 创建一个文件夹，用来保存所有的图片
if not os.path.exists('./qiutuLibs'):
    os.mkdir('./qiutuLibs')
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36 Edg/87.0.664.75'
}

# 设置一个通用url模板
url = 'https://www.qiushibaike.com/imgrank/page/%d/'
for pageNum in range(1, 3):
    new_url = format(url % pageNum)

    # 使用通用爬虫对url对应的一整张页面进行爬取
    page_text = requests.get(url=new_url, headers=headers).text

    # 使用聚焦爬虫将页面中所有的糗图进行解析
    ex = '<div class="thumb">.*?<img src="(.*?)" alt.*?</div>'
    img_src_list = re.findall(ex, page_text, re.S)
    # print(img_src_list)
    for src in img_src_list:
        # 拼接出一个完整的url
        src = 'https:' + src
        # 请求到了图片的二进制数据
        img_data = requests.get(url=src, headers=headers).content
        # 生成图片名称
        img_name = src.split('/')[-1]
        # 图片存储的路径
        img_path = './qiutuLibs/'+img_name
        with open(img_path, 'wb') as fp:
            fp.write(img_data)
            print(img_name, '下载成功！！')
```

> ![](https://gitee.com/yao_zhimin/myimg/raw/master/20210117151151.png)

## 2. bs4(python独有)

### 2.1 基础知识

+ bs4数据解析的原理：
  1. 实例化一个BeautifulSoup对象，并且将页面源码数据加载到该对象中
  2. 通过调用BeautifulSoup对象中相关的属性或者方法进行标签定位和数据提取

+ 环境安装
  1. pip install bs4
  2. pip install lxml

+ 如何实例化一个BeautifulSoup对象

  1. from bs4 import BeautifulSoup

  2. 对象的实例化

     1. 将本地的html文档中的数据加载到该对象中

     ```python
     fp = open('day02/sogou.html', 'r', encoding='utf-8')
     soup = BeautifulSoup(fp, 'lxml')
     ```

     2. 将互联网上获取的页面源码加载到该对象中

     ```python
     page_text = response.text
     soup = BeautifulSoup(page_text,'lxml')
     ```

+ bs4中提供的用于数据解析的方法和属性

  1. soup方法

     1. soup.tagName:返回的是文档中第一次出现的tagName对应的标签
     2. soup.find()
        1. soup.find('tagName')等用于soup.tagName
        2. 属性定位：soup.find('div', class_/id/attr='song')
     3. soup.find_all('tagName'):返回符合要求的所有标签(列表)

  2. select方法

     1. select('某种选择器(id,class,标签...)')：返回的是一个列表
     2. 层级选择器
        1. soup.select('.tang >ul >li > a')[0] ：>表示的是一个层级
        2. soup.select('.tang >ul a')[0] ：空格表示的是多个层级

  3. 获取标签之间的文本数据：soup.a.text/string/get_text()

     > text/get_text():可以获取某一个标签中所有的文本内容
     >
     > string：只可以获取该标签下面直系的文本内容

  4. 获取标签中属性值：soup.a['href']

### 2.2 案例

+ 爬取三国演义小说所有章节标题和内容

```python
import requests
from bs4 import BeautifulSoup
import sys
import time
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36 Edg/87.0.664.75'
}
url = 'https://www.shicimingju.com/book/sanguoyanyi.html'
page_text = requests.get(url=url, headers=headers).text
# 在首页中解析出章节标题和详情页url
# 1.实例化一个BeautifulSoup对象，需要将页面源码数据加载到该对象中
soup = BeautifulSoup(page_text, 'lxml')
li_list = soup.select('.book-mulu > ul > li')
fp = open('./sanguo.txt', 'w', encoding='utf-8')
for li in li_list:
    title = li.a.string  # 章节标题
    detail_url = 'https://www.shicimingju.com'+li.a['href']
    # 对详情页发起请求，解析出章节内容
    detail_page_text = requests.get(url=detail_url, headers=headers).text
    # 解析出详情页中相关的章节内容
    detail_soup = BeautifulSoup(detail_page_text, 'lxml')
    div_tag = detail_soup.find('div', class_='chapter_content')
    # 解析到章节内容
    content = div_tag.get_text()
    fp.write(title+':'+content+'\n')
    print(title, '爬取成功！！')
    time.sleep(1)	#设置休眠1秒，爬的太快容易被封ip
```

> ![](https://gitee.com/yao_zhimin/myimg/raw/master/20210117165554.png)

## 3. xpath

### 3.1 基础知识

> xpath --- 最常用，最便捷高效，通用性

+ xpath 解析原理

  1. 实例化一个etree对象，且需要将被解析的页面源码数据加载到该对象中
  2. 调用etree对象中的xpath方法结合着xpath表达式实现标签的定位和内容的捕获

+ 环境的安装

  pip install lxml

+ 如何实例化一个etree对象

  1. 将本地的html文档中的源码数据加载到etree对象中

     ​	etree.parse(filepath)

  2. 可以将从互联网上获取的源码数据加载到该对象中

     ​	etree.HTML('page_text')

  3. xpath('xpath表达式')

+ xpath表达式

  1. / 表示从根节点开始定位。表示的是一个层级。
  2. // 表示多个层级。可以表示从任意位置开始定位
  3. 属性定位：//div[@class="song"]     tag[@attrName="attrVal"]
  4. 索引定位：//div[@class="song"]/p[3]    索引从1开始
  5. 取文本：
     1. /text()	==>获取标签中直系的文本内容
     2. //text()	==>获取标签中非直系的文本内容(所有的文本内容)
  6. 取属性：
     1. /@attrName==>img/src

### 3.2 案例

+ 爬取58二手房中的房源信息

```python
import requests
from lxml import etree
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36 Edg/87.0.664.75'
}
# 爬取到页面源码数据
url = 'https://luannanxian.58.com/ershoufang/?PGTID=0d100000-01b9-adc3-c4db-03610df4b6cc&ClickID=2'
page_text = requests.get(url=url, headers=headers).text
# 数据解析
tree = etree.HTML(page_text)
li_list = tree.xpath('//ul[@class="house-list-wrap"]/li')
fp = open('58.txt', 'w', encoding='utf-8')
for li in li_list:
    # 局部解析
    title = li.xpath('./div[@class="list-info"]/h2/a/text()')[0]  # ./表示当前的li标签
    print(title)
    fp.write(title+'\n')
```

> ![](https://gitee.com/yao_zhimin/myimg/raw/master/20210118112154.png)

+ 解析下载图片数据

```python
import requests
from lxml import etree
import os
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36 Edg/87.0.664.75'
}
url = 'http://pic.netbian.com/4kfengjing/'
response = requests.get(url=url, headers=headers)
# 手动设定相应数据的编码格式
# response.encoding = 'gbk'
page_text = response.text
# 数据解析：src的属性值
tree = etree.HTML(page_text)
li_list = tree.xpath('//div[@class="slist"]//li')
# 创建一个文件夹
if not os.path.exists('./picLibs'):
    os.mkdir('./picLibs')
for li in li_list:
    img_src = 'http://pic.netbian.com'+li.xpath('./a/img/@src')[0]
    img_name = li.xpath('./a/img/@alt')[0]+'.jpg'
    # 通用处理中文乱码的解决方案
    img_name = img_name.encode('iso-8859-1').decode('gbk')
    # print(img_src, img_name)
    # 请求图片进行持久化存储
    img_data = requests.get(url=img_src, headers=headers).content
    img_path = 'picLibs/'+img_name
    with open(img_path, 'wb') as fp:
        fp.write(img_data)
        print(img_name+'下载成功')
```

> ![](https://gitee.com/yao_zhimin/myimg/raw/master/20210118131716.png)

+ 全国城市名称爬取

```python
import requests
from lxml import etree
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36 Edg/87.0.664.75'
}
url = 'https://www.aqistudy.cn/historydata/'
page_text = requests.get(url=url, headers=headers).text
# tree = etree.HTML(page_text)
# hot_li_list = tree.xpath('//div[@class="bottom"]/ul/li')
# all_city_names = []
# # 解析到热门城市的城市名称
# for li in hot_li_list:
#     hot_city_name = li.xpath('./a/text()')[0]
#     all_city_names.append(hot_city_name)
# city_names_list = tree.xpath('//div[@class="bottom"]/ul/div[2]/li')
# # 解析到全部城市的城市名称
# for li in city_names_list:
#     city_name = li.xpath('./a/text()')[0]
#     all_city_names.append(city_name)
# print(all_city_names, len(all_city_names))

tree = etree.HTML(page_text)
# 解析到热门城市和所有城市对应的a标签
# //div[@class="bottom"]/ul/li/a   //div[@class="bottom"]/ul/div[2]/li/a
a_list = tree.xpath(
    '//div[@class="bottom"]/ul/li/a | //div[@class="bottom"]/ul/div[2]/li/a')
all_city_names = []
for a in a_list:
    city_name = a.xpath('./text()')[0]
    all_city_names.append(city_name)
print(all_city_names, len(all_city_names))
```

> ['北京', '上海', '广州', '深圳', '杭州', '天津', '成都', '南京', '西安', '武汉', '阿坝州', '安康', '阿克苏地区', '阿里地区', '阿拉善盟', '阿勒泰地区', '安庆', '安顺', '鞍山', '克孜勒苏州', '安阳', '蚌埠', '白城', '保定', '北海', '宝鸡', '北京', '毕节', '博州', '白山', '百色', '保山', '白沙', '包头', '保亭', '本溪', '巴彦淖尔', '白银', '巴中', '滨州', '亳州', '长春', '昌都', '常德', '成都', '承德', '赤峰', '昌吉州', '五家渠', '昌江', '澄迈', '重庆', '长沙', '常熟', '楚雄州', '朝阳', '沧州', '长治', '常州', '潮州', '郴州', '池州', '崇左', '滁州', '定安', '丹东', '东方', '东莞', '德宏州', '大理州', '大连', '大庆', '大同', '定西', '大兴安岭地区', '德阳', '东营', '黔南州', '达州', '德州', '儋州', '鄂尔多斯', '恩施州', '鄂州', '防城港', '佛山', '抚顺', '阜新', '阜阳', '富阳', '抚州', '福州', '广安', '贵港', '桂林', '果洛州', '甘南州', '固原', '广元', '贵阳', '甘孜州', '赣州', '广州', '淮安', '海北州', '鹤壁', '淮北', '河池', '海东地区', '邯郸', '哈尔滨', '合肥', '鹤岗', '黄冈', '黑河', '红河州', '怀化', '呼和浩特', '海口', '呼伦贝尔', '葫芦岛', '哈密地区', '海门', '海南州', '淮南', '黄南州', '衡水', '黄山', '黄石', '和田地区', '海西州', '河源', '衡阳', '汉中', '杭州', '菏泽', '贺州', '湖州', '惠州', '吉安', '金昌', '晋城', '景德镇', '金华', '西双版纳州', '九江', '吉林', '即墨', '江门', '荆门', '佳木斯', '济南', '济宁', '胶南', '酒泉', '句容', '湘西州', '金坛', '鸡西', '嘉兴', '江阴', '揭阳', '济源', '嘉峪关', '胶州', '焦作', '锦州', '晋中', '荆州', '库尔勒', '开封', '黔东南州', '克拉玛依', '昆明', '喀什地区', '昆山', '临安', '六安', '来宾', '聊城', '临沧', '娄底', '乐东', '廊坊', '临汾', '临高', '漯河', '丽江', '吕梁', '陇南', '六盘水', '拉萨', '乐山', '丽水', '凉山州', '陵水', '莱芜', '莱西', '临夏州', '溧阳', '辽阳', '辽源', '临沂', '龙岩', '洛阳', '连云港', '莱州', '兰州', '林芝', '柳州', '泸州', '马鞍山', '牡丹江', '茂名', '眉山', '绵阳', '梅州', '宁波', '南昌', '南充', '宁德', '内江', '南京', '怒江州', '南宁', '南平', '那曲地区', '南通', '南阳', '平度', '平顶山', '普洱', '盘锦', '蓬莱', '平凉', '莆田', '萍乡', '濮阳', '攀枝花', '青岛', '琼海', '秦皇岛', '曲靖', '齐齐哈尔', '七台河', '黔西南州', '清远', '庆阳', '钦州', '衢州', '泉州', '琼中', '荣成', '日喀则', '乳山', '日照', '韶关', '寿光', '上海', '绥化', '石河子', '石家庄', '商洛', '三明', '三门峡', '山南', '遂宁', '四平', '商丘', '宿迁', '上饶', '汕头', '汕尾', '绍兴', '三亚', '邵阳', '沈阳', '十堰', '松原', '双鸭山', '深圳', '朔州', '宿州', '随州', '苏州', '石嘴山', '泰安', '塔城地区', '太仓', '铜川', '屯昌', '通化', '天津', '铁岭', '通辽', '铜陵', '吐鲁番地区', '铜仁地区', '唐山', '天水', '太原', '台州', '泰州', '文昌', '文登', '潍坊', '瓦房店', '威海', '乌海', '芜湖', '武汉', '吴江', '乌兰察布', '乌鲁木齐', '渭南', '万宁', '文山州', '武威', '无锡', '温州', '吴忠', '梧州', '五指山', '西安', '兴安盟', '许昌', '宣城', '襄阳', '孝感', '迪庆州', '锡林郭勒盟', '厦门', '西宁', '咸宁', '湘潭', '邢台', '新乡', '咸阳', '新余', '信阳', '忻州', '徐州', '雅安', '延安', '延边州', '宜宾', '盐城', '宜昌', '宜春', '银川', '运城', '伊春', '云浮', '阳江', '营口', '榆林', '玉林', '伊犁哈萨克州', '阳泉', '玉树州', '烟台', '鹰潭', '义乌', '宜兴', '玉溪', '益阳', '岳阳', '扬州', '永州', '淄博', '自贡', '珠海', '湛江', '镇江', '诸暨', '张家港', '张家界', '张家口', '周口', '驻马店', '章丘', '肇庆', '中山', '舟山', '昭通', '中卫', '张掖', '招远', '资阳', '遵义', '枣庄', '漳州', '郑州', '株洲'] 394

+ 爬取站长素材中免费简历模板

```python
import requests
from lxml import etree
import os
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36 Edg/87.0.664.75'
}
# 分页操作
for pageNum in range(1, 6):  # 下载前5页
    if pageNum == 1:
        url = 'https://sc.chinaz.com/jianli/free.html'
    else:
        url = format('https://sc.chinaz.com/jianli/free_%d.html' % pageNum)
    page_text = requests.get(url=url, headers=headers).text
    tree = etree.HTML(page_text)
    # 获取简历链接和名称
    div_list = tree.xpath('//div[@class="sc_warp  mt20"]/div/div/div')
    # print(div_list)
    if not os.path.exists('./jianliLibs'):
        os.mkdir('./jianliLibs')
    for div in div_list:
        jianli_name = div.xpath('./a/img/@alt')[0]+'.rar'
        # 通用处理中文乱码的解决方案
        jianli_name = jianli_name.encode('iso-8859-1').decode('utf-8')
        jianli_href = 'https:'+div.xpath('./a/@href')[0]
        # 下载简历
        jianli_detail_text = requests.get(
            url=jianli_href, headers=headers).text
        jianli_tree = etree.HTML(jianli_detail_text)
        jianli_url = jianli_tree.xpath(
            '//div[@class="down_wrap"]/div[2]/ul/li/a/@href')[0]
        jianli_data = requests.get(url=jianli_url, headers=headers).content
        jianli_path = 'jianliLibs/'+jianli_name
        with open(jianli_path, 'wb') as fp:
            fp.write(jianli_data)
            print(jianli_name+'下载成功')
```

> ![](https://gitee.com/yao_zhimin/myimg/raw/master/20210118131217.png)

