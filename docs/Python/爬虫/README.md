# 一、爬虫基础简介

## 1.爬虫相关概念

> + 爬虫的概念：通过编写程序，模拟浏览器上网，然后让其去互联网上抓取数据的过程
> + 爬虫的合法性：
>   + 在法律中是不被禁止的
>   + 具有违法风险：干扰了被访问网站的正常运营；抓取了受到法律保护的特定类型的数据或信息
> + 爬虫的分类
>   + 通用爬虫：抓取系统重要组成部分，抓取的是一整张页面数据
>   + 聚焦爬虫：建立在通用爬虫基础之上，抓取的是页面中特定的局部内容
>   + 增量式爬虫：检测网站中数据更新的情况，只会抓取网站中最新更新出来的数据
> + 反爬机制：门户网站可以通过制定相应的策略或者技术手段，防止爬虫程序进行网站数据的爬取
> + 反反爬策略：爬虫程序可以通过指定相关的策略或者技术手段，破解门户网站中具备的反爬机制，从而可以获取门户网站中相关的数据
> + robots.txt协议(君子协议)：规定了网站中哪些数据可以被爬虫爬取，哪些数据不可以被爬取

## 2. http & https 协议

> + http协议：就是服务器和客户端进行数据交互的一种形式
>   + 常用请求头信息：
>     + User-Agent：请求载体的身份标识
>     + Connection：请求完毕后，是断开连接还是保持连接
>   + 常用响应头信息
>     + Content-Type：服务器响应回客户端的数据类型
> + https协议：安全的超文本传输协议
>   + 加密方式
>     + 对称秘钥加密
>     + 非对称秘钥加密
>     + 整数秘钥加密

# 二、requests模块

> + python中原生的一款基于网络请求的模块，功能强大，简单便捷，效率极高
> + 作用：模拟浏览器发请求
> + requests模块的使用(编码流程)
>   1. 指定url
>   2. 发起请求
>   3. 获取相应数据
>   4. 持久化存储

+ 爬取搜狗首页信息

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
> UA伪装：让爬虫对应的请求载体身份标识伪装成某一款浏览器

+ 网页采集器

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

+ 破解百度翻译

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

+ 爬取豆瓣电影分类排行榜

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

+ 爬取肯德基餐厅查询地点

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

+ 爬取化妆品企业详情信息

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

+ 正则
+ bs4
+ xpath