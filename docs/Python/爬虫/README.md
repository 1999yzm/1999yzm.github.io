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

### 1.1 正则表达式基础知识

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

## 2. bs4

## 3. xpath

