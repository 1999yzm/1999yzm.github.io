# 

# 一、注释的使用

- 单行注释

```python
# 单行注释，只对本行有效
```

- 多行注释

```python
#第一种
'''
三个单引号开始，三个单引号结束，中间的内容是注释
里面允许换行
'''
#第二种
"""
这也是多行注释
三个双引号开始，三个双引号结束
"""
```

# 二、变量和数据类型

> **标识符和关键字**
>
> 1. 标识符：变量、模块名、函数名、类名....
>
> 2. 标识符的命名规则：
>
>    1. 由数字、字母和\_组成，且不能以数字开头
>    2. 严格区分大小写
>    3. 不能使用关键字（在 python 里有特殊含义）
>
>    python 关键字：![](https://gitee.com/yao_zhimin/myimg/raw/master/20210108161545.png)
>
> 3. 标识符命名规范
>    1. 顾名思义
>    2. 命名规范:
>       1. 小驼峰法：userNameAndPassword
>       2. 大驼峰法（类名）:UserName
>       3. 使用下划线连接：user_name

```python
#在python里，变量是没有数据类型的，我们所说变量的数据类型，其实是变量所对应的值的数据类型
#查看数据类型--使用type内置类
a = 34
b = 'hello'
c = True
d = ['name1','name2','name3']
print(type(a))	#<class 'int'>
print(type(b))	#<class 'str'>
print(type(c))	#<class 'bool'>
print(type(d))	#<class 'list'>
```

## 1.数字类型(numbers)

- 整形（int）

```python
#十进制(默认)
a = 98
#二进制bin
#print(bin(a))
a = 0b1100010
#一个二进制位是一位(bit)
#1字节(Byte)=8位(bit)
#1KB( Kilobyte，千字节)=1024B
#1MB( Megabyte，兆字节)=1024KB
#八进制oct
#print(oct(a))
a = 0o142
#十六进制hex
#print(hex(a))
a = 0x62
```

- 浮点型（float）
- 复数（complex）

## 2.布尔类型(bool)

```python
#在计算机里，True用1表示，False用0表示
print(True+1)	#2
print(False+1)	#1
```

## 3.字符串类型(str)

- 字符串的表示方式

```python
a = 'hello'  #可以用一对单引号表示
b = "hello"  #可以用一对双引号表示
c = """hello"""  #可以用一对三个双引号表示(有赋值时是字符串，否则是多行注释)
d = '''hello'''  #可以用一对三个单引号表示(有赋值时是字符串，否则是多行注释)

#转义字符
# \'==>显示一个普通单引号	\"==>显示一个普通的双引号	\\==>显示一个普通的反斜线
# \n==>表示一个换行	\t==>表示一个制表符

#在字符串的前面添加 r 在python里表示的是原生字符串
a = r'hello \teacher'
print(a)	#hello \teacher
```

- 下标和切片

```python
#下标又称为索引,表示第几个数据,下标都是从0开始
str = 'zhangsan'	#字符串：一个一个的字符‘串’在一起
#可以通过下标来获取指定位置的数据
print(str[4])	# g
#字符串是不可变的数据类型,对于字符串的任何操作，都不会改变原有的字符串
#str[4] = 'x'	#报错

#切片就是从字符串里复制一段指定的内容，生成一个新的字符串
str = 'abcdefghijklmnopqrstuvwxyz'
print(str[5])	#==>获取指定下标上的数据
#切片语法:  str[start:end:step]
#顾头不顾尾
#step表示步长，可以理解为间隔(默认为1)，不能为0,
#step可以是负数,表示从右往左复制
print(str[2:9])	#cdefghi
print(str[2:])	#只设置start，会‘截取’到最后
print(str[:9])	#只设置end，会从头开始‘截取’
print(str[3:15])	#defghijklmno
print(str[3:15:2])	#dfhjln
print(str[15:3:-1])	#ponmlkjihgfe
print(str[::])	#abcdefghijklmnopqrstuvwxyz，即从头到尾复制
print(str[::-1])	#zyxwvutsrqponmlkjihgfedcba
print(str[-9:-5:2])		#rt
```

- 常见操作

```python
str = 'abcdefghijklmnlaeghliae'

#获取字符串的长度
print(len(str))	#14

#查找内容相关的方法(获取指定字符的下标)	find/index/rfind/rindex
print(str.find('l'))	#11(最小下标)
print(str.rfind('l'))	#19(最大下标)
print(str.find('z'))	#找不到则返回-1
print(str.find('l',4,9))	#-1
print(str.index('l'))	#11(最小下标)
print(str.rindex('l'))	#19(最大下标)
print(str.index('z'))	#找不到则报错

#字符串判断操作，结果是一个布尔类型	startswith,endswith,isalpha,isdigit,isalnum,isspace
print('hello'.startswith('he'))	#True
print('hello'.endswith('o'))	#True
print('hello'.isalpha())	#True--判断是否是字母
print('hello'.isdigit())	#False--判断是否是数字(整数)
print('hello'.isalnum())	#True--判断是否由字母或数字组成
print('hello'.isspace())	#False--判断是否全部由空格组成

#replace方法:用来替换字符串
str = 'hello'
print(str)	#hello	字符串是不可变数据类型
print(str.replace('l','x'))	#hexxo	原来的字符串不变，而是生成一个新的字符串来保存替换后的结果

#内容分隔相关操作	split splitlines partition rpartition
str = 'zhangsan,lisi,wangwu,jerry,henry,merry,jack,tony'

#使用split方法，可以将一个字符串切割成一个列表
print(str.split(','))	#['zhangsan,lisi,wangwu,jerry,henry,merry,jack,tony']
print(str.rsplit(','))	#['zhangsan,lisi,wangwu,jerry,henry,merry,jack,tony']
print(str.split(',',2))	#['zhangsan', 'lisi', 'wangwu,jerry,henry,merry,jack,tony']
print(str.rsplit(',',2))	#['zhangsan,lisi,wangwu,jerry,henry,merry', 'jack', 'tony']

#使用splinelines可以进行换行分隔
str = 'hello \n world'
print(str.splitlines())	#['hello ', ' world']

#partition 指定一个字符串作为分隔符，分为三部分:前面、分隔符、后面
print('ooaihfoahgUaoeghoi'.partition('U'))	#('ooaihfoahg', 'U', 'aoUeghoi')
print('ooaihfoahgUaoUeghoi'.rpartition('U'))	#('ooaihfoahgUao', 'U', 'eghoi')

#将列表转换成为字符串--join方法,参数为一个可迭代对象
str = ['apple', 'pear', 'peach', 'banana', 'orange', 'grape']
print('-'.join(str))    #apple-pear-peach-banana-orange-grape
print('*'.join('hello'))	#h*e*l*l*o

#字符串格式
#capitalize--让第一个单词的首字母大写
print('hello \n world'.capitalize())	#Hello world
#upper--全大写
print('hello world'.upper())  # HELLO WORLD
#lower--全小写
print('hello WOrld'.lower())  # hello world
#title--每个单词的首字母大写
print('hello WOrld'.title())  # Hello World
#ljust(length,fillchar)
print('Monday'.ljust(10, '+'))	#Monday++++
#rjust(length,fillchar)
print('Monday'.rjust(10, '-'))	#----Monday
#center(length,fillchar)
print('Monday'.center(20, '*'))	#*******Monday*******
#strip--去掉字符，默认为去掉空格
print('+++++Monday+++++'.lstrip('+'))    #Monday+++++
print('+++++Monday+++++'.rstrip('+'))    #+++++Monday
print('+++++Monday+++++'.strip('+'))	#Monday
```

- 字符集

> 1. 首先出现了 ASCII 码表，只用一个字节的低 7 位来表示一个字符
>
> ![](https://gitee.com/yao_zhimin/myimg/raw/master/20210108181507.png)
>
> 2. 然后出现了 ISO-8859-1(Latin 1)表，用上了最高位，0~127 和 ASCII 码表完全兼容，最多表示 255 个
>
> <img src="https://gitee.com/yao_zhimin/myimg/raw/master/20210108182307.png" style="zoom:;" />
>
> 3. 再然后出现了 Unicode 编码(万国码)==>绝大部分国家的文字都有一个对应的编码

```python
#使用内置函数 chr 和 ord 能够查看数字和字符的对应关系
print(ord('ま'))  # 12414
print(chr(97))  # a
```

> - GBK(国标扩)：汉字占两个字节，简体中文
> - BIG5：繁体中文
> - utf-8：统一编码，汉字占三个字节

```python
#使用encode方法将字符串转换为指定编码集结果
print('你'.encode('gbk'))  # b'\xc4\xe3'	两个字节
print('你'.encode('utf-8'))  # b'\xe4\xbd\xa0'	三个字节

#使用decode方法将一个编码集结果转换为对应的字符
str = b'\xe4\xbd\xa0'
print(str.decode('utf-8'))  #你

#为什么会出现乱码？
str = '你好'.encode('utf-8')
print(str)  #b'\xe4\xbd\xa0  \xe5\xa5\xbd'
print(str.decode('gbk'))    #浣犲ソ \xe4\xbd  xa0\ xe5  \xa5\xbd
```

- 格式化打印字符串

```python
#可以使用 % 占位符来表示格式化一个字符串
#  %s==>表示的是字符串的占位符
#  %d==>表示的是整数的占位符
#	%nd==>打印时，显示n位，如果不够，在前面使用空格补齐
#  %x==>表将数字使用十六进制输出
#  %f==>表示的是浮点数的占位符
#  %.nf==>保留小数点后n位
#  %%==>输出一个%
name = 'zhangsan'
age = 18
print('大家好,我的名字是', name, '我今年', age, '岁了', sep='')  # 大家好,我的名字是zhangsan我今年18岁了
print('大家好,我的名字是%s我今年%d岁了' % (name, age))  # 大家好,我的名字是zhangsan我今年18岁了

#format方法的使用
print('大家好，我是{},我今年{}岁了'.format('张三',18))	#大家好，我是张三,我今年18岁了
print('大家好，我是{1},我今年{0}岁了'.format(18,'张三'))	#大家好，我是张三,我今年18岁了
print('大家好，我是{name},我今年{age}岁了'.format(age=18,name='张三'))	#大家好，我是张三,我今年18岁了

age = 18
name = 'zhangsan'
print(f"大家好，我是{name},我今年{age}岁了")	#大家好，我是zhangsan,我今年18岁了
```

+ 正则表达式

```python

```

## 4.列表类型(list)

- 基本使用

```python
#当我们有多个数据需要按照一定的顺序保存的时候，我们可以考虑列表
#列表是有序可变的数据类型
#表示方法
names = ['韩信','关羽','张飞','黄忠','蔡文姬','刘备']
names = list(('韩信','关羽','张飞','黄忠','蔡文姬','刘备'))

#列表推导式
nums = [i for i in range(10) if i % 2]
print(nums)  # [1, 3, 5, 7, 9]

names = ['韩信', '关羽', '张飞', '黄忠', '蔡文姬', '刘备']
# 取值
print(names[4])  # 蔡文姬
# 切片(就是一个浅拷贝)
print(names[3:])	#['黄忠', '刘禅', '刘备']
```

- 列表操作(增、删、改、查)

```python
heros = ['韩信', '关羽', '张飞', '黄忠', '蔡文姬', '刘备']
# 增  append insert extend
heros.append('阿珂')	# ['韩信', '关羽', '张飞', '黄忠', '蔡文姬', '刘备', '阿珂']
heros.insert(3, '李白')	# ['韩信', '关羽', '张飞', '李白', '黄忠', '蔡文姬', '刘备', '阿珂']
x = ['马可波罗', '狄仁杰', '后羿']
heros.extend(x)	# ['韩信', '关羽', '张飞', '李白', '黄忠', '蔡文姬', '刘备', '阿珂', '马可波罗', '狄仁杰', '后羿']

#删 pop remove clear
#pop(index)--删除指定位置上的数据，默认会删除列表里最后一个数据，并且返回这个数据
heros.pop()	#['韩信', '关羽', '张飞', '李白', '黄忠', '蔡文姬', '刘备', '阿珂', '马可波罗', '狄仁杰']
#remove--删除指定元素，不存在则报错
heros.remove('黄忠')	# ['韩信', '关羽', '张飞', '李白', '蔡文姬', '刘备', '阿珂', '马可波罗', '狄仁杰']
#clear--用来清空一个列表
heros.clear()	#[]

#改
heros = ['韩信', '关羽', '张飞', '黄忠', '蔡文姬', '刘备', '张飞']
heros[3] = 'kai'	#['韩信', '关羽', '张飞', 'kai', '蔡文姬', '刘备', '张飞']

#查
#index--找下标，不存在则报错
heros = ['韩信', '关羽', '张飞', '黄忠', '蔡文姬', '刘备', '张飞']
print(heros.index('张飞'))  # 2
#count--用来计数
print(heros.count('张飞'))  # 2
#in 运算符
print('黄忠' in heros)	#True
```

- 列表复制

```python
#可变类型和不可变类型
#	不可变类型(如果修改值，内存地址会发生变化)：字符串、数字、元组
#	可变类型(如果修改值，内存地址不会发生变化)：列表、字典、集合
x = [100,200,300]
y = x	# 等号是内存地址的赋值
z = x.copy()	#调用copy方法，可以复制一个列表，这个列表和原有的列表之间内容一样，但是指向不同的内存空间(浅拷贝)
print(f"{hex(id(x))},{hex(id(y))},{hex(id(z))}")
#0x16f37668600,0x16f37668600,0x16f376a7600

#还可以使用copy模块实现拷贝
import copy
a = copy.copy(x)	#效果等价于x.copy(),都是浅拷贝

##浅拷贝(只拷贝了一层)
str = ['hello', 'good', [100, 200, 300], 'yes', 'hi', 'ok']
str1 = str.copy()  # str1是str的浅拷贝
str[0] = '你好'
print(str)  #['你好', 'good', [100, 200, 300], 'yes', 'hi', 'ok']
print(str1) #['hello', 'good', [100, 200, 300], 'yes', 'hi', 'ok']
str1[2][0] = 1
print(str)  # ['你好', 'good', [1, 200, 300], 'yes', 'hi', 'ok']
print(str1)  # ['你好', 'good', [1, 200, 300], 'yes', 'hi', 'ok']

##深拷贝(只能使用copy模块实现)##
import copy
str = ['hello', 'good', [100, 200, 300], 'yes', 'hi', 'ok']
str2 = copy.deepcopy(str)  # str2是str的深拷贝
str[0] = '你好'
print(str)  # ['你好', 'good', [100, 200, 300], 'yes', 'hi', 'ok']
print(str2)  # ['hello', 'good', [100, 200, 300], 'yes', 'hi', 'ok']
str2[2][0] = 1
print(str)  # ['你好', 'good', [100, 200, 300], 'yes', 'hi', 'ok']
print(str2)  # ['hello', 'good', [1, 200, 300], 'yes', 'hi', 'ok']
```

- 列表的遍历

```python
heros = ['韩信', '关羽', '张飞', '黄忠', '蔡文姬', '刘备', '张飞']
#for...in.. 循环遍历
for hero in heros:
    print(hero)

#while 循环遍历
count = 0
while count < len(heros):
    print(heros[count])
    count += 1
```

- 排序算法

```python
#交换两个变量的值
a = 12
b = 15
a,b = b,a
print(a,b)	#15 12

#sort方法--直接对nums进行排序
nums = [6, 5, 3, 1, 8, 7, 2, 4]
print(nums.sort())	#[1, 2, 3, 4, 5, 6, 7, 8]
nums.sort(reverse=True)
print(nums) #[8, 7, 6, 5, 4, 3, 2, 1]
#sorted方法--排序后生成新的列表，原列表不变
print(sorted(nums)) #[1, 2, 3, 4, 5, 6, 7, 8]

#reverse方法--列表反转
nums = [1, 2, 3, 4, 5, 6, 7, 8]
nums.reverse()
print(nums)  # [8, 7, 6, 5, 4, 3, 2, 1]

#冒泡排序
nums = [6, 5, 3, 1, 8, 7, 2, 4]
for i in range(0, len(nums)-1):
    for j in range(0, len(nums)-i-1):
        if nums[j] > nums[j+1]:
            nums[j], nums[j+1] = nums[j+1], nums[j]
for num in nums:
    print(num, end=' ')
```

- 列表的嵌套

```python
import random
teachers = ['A','B','C','D','E','F']
rooms = [[],[],[]]
for teacher in teachers:
    room = random.choice(rooms)
    room.append(teacher)
print(rooms)
```

## 5.元组类型(tuple)

- 基本用法

```python
#元组和列表很像，都是用来保存多个数据，也是一个有序的存储数据的容器
#使用一对小括号 () 来表示一个元组
#元组和列表的区别在于，列表是可变的，而元组是不可变数据类型
words = ['hello','yes','good','hi']	#列表
nums = (8,34,54,64,2,3,0,3,2,0)	#元组

#只有一个元素的元组
ages = (18,)	#<class 'tuple'>

# 通过下标获取元素
print(nums[3])  # 64

# 不能对元组进行增加和修改操作

# 元组查找
print(nums.index(2))  # 4
print(nums.count(3))  # 2

#列表转换为元组
print(tuple(words))	#('hello', 'yes', 'good', 'hi')
#元组转换为列表
print(list(nums))	#[8, 34, 54, 64, 2, 3, 0, 3, 2, 0]

#遍历元组
for i in nums:
    print(i)
```

## 6.字典类型(dict)

- 字典(可变数据类型)的基本使用

> - 列表只能存储值，但是无法对值进行描述
> - 字典不仅可以保存值，还能进行描述

```python
#使用大括号来表示一个字典，不仅有值value，还有值的描述key
#字典里的数据都是以键值对key-value的形式保存的
#key和value之间使用冒号 : 来连接,多个键值对之间使用逗号 , 来分隔

#1.字典里的key不允许重复，如果key重复了，后一个key对应的值会覆盖前一个
#2.字典里的value可以是任意数据类型，但是key只能使用不可变数据类型，一般使用字符串
person = {'name':'zhangsan',
          'age':18,
          'height':180,
          'age':20,	#会覆盖前一个age
          'hobbies':['唱','跳','rap'],
}

#字典推导式
dict1 = {
    'a':100,
    'b':200,
    'c':300
}
dict1 = {v:k for k,v in dict1.items()}
print(dict1)	#{100: 'a', 200: 'b', 300: 'c'}
```

- 字典的增删改查

```python
person = {
    'name': 'zhangsan',
    'age': 18
}

# 查(字典的数据在保存时，是无序的，不能通过下标来获取)
# 使用key获取到对应的value，如果要查找的key不存在，会直接报错
print(person['name'])	#zhangsan
# 使用get方法，如果key不存在，使用户给点的默认值，默认返回None，而不报错
print(person.get('height'))  # None
print(person.get('gender','female'))	#female

# 改、增
# 直接使用key可以修改对应的value
# 如果key存在，则修改所对应的value值，
# 如果key不存在，会往字典里添加一个新的key-value对
person['name'] = 'lisi'
person['gender'] = 'female'
print(person)   #{'name': 'lisi', 'age': 18, 'gender': 'female'}

# 删
# pop方法--执行结果返回被删除元素的value值
person.pop('name')
print(person)	#{'age': 18, 'gender': 'female'}
# popitem方法--删除一个元素，结果是被删除的这个元素组成的键值对
person = {
    'name': 'zhangsan',
    'age': 18
}
result = person.popitem()
print(result)  # ('age', 18)
print(person)  # {'name': 'zhangsan'}
# clear方法--清空字典
person = {
    'name': 'zhangsan',
    'age': 18
}
person.clear()
print(person)   #{}
# del方法
person = {
    'name': 'zhangsan',
    'age': 18
}
del person['name']
print(person)  # {'age': 18}
```

- update 方法

> - 列表可以使用 extend 方法将两个列表合并成为一个列表
> - 字典中可以使用 update 方法实现类似操作

```python
person1 = {
    'name': 'zhangsan',
    'age': 18
}
person2 = {
    'height': 180,
    'weight': 130
}
person1.update(person2)
print(person1)  # {'name': 'zhangsan', 'age': 18, 'height': 180, 'weight': 130}
```

- 字典的遍历

```python
person = {
    'name': 'zhangsan',
    'age': 18,
    'height': 180,
    'weight': 130
}

# 第一种遍历方式:for...in
for x in person:  # for...in循环获取的是key
    print(x, '=', person[x])  # name = zhangsan age = 18 height = 180 weight = 130

#第二种方式:获取到所有的key，然后再遍历key，根据key获取value
for k in person.keys():
    print(k,'=',person[k])  # name = zhangsan age = 18 height = 180 weight = 130

#第三种方式:获取到所有的value,只能拿到value，不能拿到key
for v in person.values():
    print(v)	#zhangsan 18 180 130

#第四种方式:
for k,v in person.items():
    print(k,'=',v)	#name = zhangsan\age = 18\height = 180\weight = 130
```

## 7.集合类型(set)

> 集合特征：不重复、无序；可以使用 {} 或者 set 来表示
>
> {}--里边放的是键值对，它就是一个字典
>
> {}--里边放的是单个的值，它就是一个集合

- 集合的基本使用

```python
#如果有重复的数据，会自动去重
names = {'zhangsan','lisi','jack','tony','jack','lisi'}
print(names)	#{'jack', 'lisi', 'tony', 'zhangsan'}
```

- 集合的增删改查(一般不用)

```python
#增
names = {'zhangsan','lisi','jack','tony','jack','lisi'}
#add方法
names.add('甄姬')
print(names)	#{'zhangsan', 'lisi', '甄姬', 'tony', 'jack'}
#union方法--合并形成一个新的集合
names.union({'张三','李四'})
print(names)	#{'jack', 'tony', 'zhangsan', 'lisi'}
print(names.union({'张三','李四'}))	#{'jack', '张三', 'zhangsan', '李四', 'tony', 'lisi'}
#A.update(B)--将B拼接到A
names.update({'张三','李四'})
print(names)	#{'tony', 'jack', 'zhangsan', '李四', '张三', 'lisi'}

#删
#clear方法--清空集合  {}--空集合   set()--空字典
names = {'zhangsan','lisi','jack','tony','jack','lisi'}
names.clear()
print(names)	#set()
#pop方法--随机删除一个
names = {'zhangsan','lisi','jack','tony','jack','lisi'}
names.pop()
print(names)	#{'lisi', 'tony', 'zhangsan'}
#remove方法--删除一个指定元素，如果不存在，会报错
names = {'zhangsan','lisi','jack','tony','jack','lisi'}
names.remove('lisi')
print(names)	#{'zhangsan', 'tony', 'jack'}

#没有查找操作
```

- 集合的高级使用

> set 支持很多算数运算符

```python
# '-' 求差集 A-B
a = {'1', '3', '4', '5', '6', '7', '8', '2'}
b = {'1', '3', '4', '5'}
print(a-b)	#{'7', '8', '2', '6'}

# '&' 求交集  A∩B
a = {'1', '3', '4', '5', '6', '7', '8', '2'}
b = {'1', '3', '4', '5'}
print(a & b)  # {'1', '5', '3', '4'}

# '|' 求并集 A∪B
a = {'1', '3', '4', '5', '6', '7', '8', '2'}
b = {'1', '3', '4', '5'}
print(a | b)  # {'4', '3', '6', '1', '8', '7', '5', '2'}

# '^' 求A、B差集的并集
a = {'1', '3', '4', '5', '6', '7', '8', '2'}
b = {'1', '3', '4', '5', '15'}
print(a ^ b)  # {'6', '7', '15', '8', '2'}
```

> 去重排序

```python
nums = {'1', '3', '4', '1', '3', '7', '8', '2'}
x = list(set(nums))
x.sort(reverse=True)
print(x)  # ['8', '7', '4', '3', '2', '1']
```

> 转换的相关方法

```python
nums = ['8', '7', '4', '3', '2', '1']

x = tuple(nums)	#使用tuple内置类转换为元组
print(x)	#('8', '7', '4', '3', '2', '1')

y = set(x)	#使用set内置类转换为集合
print(y)  # {'1', '4', '2', '3', '7', '8'}
```

## 公共方法总结

> \+ :可以用来拼接，用于 字符串、元组、列表

```python
print('hello'+'world')  # helloworld
print(('good', 'yes')+('hi', 'ok'))  # ('good', 'yes', 'hi', 'ok')
print([1, 2, 3]+[4, 5, 6])  # [1, 2, 3, 4, 5, 6]
```

> \- :只能用于集合，求差集

```python
print({1, 2, 3}-{3})  # {1, 2}
```

> \* :可以用于字符串元组列表，表示重复多次。(不能用于字典和集合)

```python
print('hello'*3)  # hellohellohello
print([1, 2, 3]*3)  # [1, 2, 3, 1, 2, 3, 1, 2, 3]
print((1, 2, 3)*3)  # (1, 2, 3, 1, 2, 3, 1, 2, 3)
```

> in : 成员运算符

```python
print('a' in 'abc')  # True
print(1 in [1, 2, 3])  # True
print(4 in (6, 4, 5))  # True
print(3 in {3, 4, 5})  # True
#in 用于字典用来判断可以是否存在
print('zhangsan' in {'name': 'zhangsan',
                     'age': 18, 'height': 180, 'weight': 130})  # False
print('name' in {'name': 'zhangsan', 'age': 18,
                 'height': 180, 'weight': 130})  # True
```

> 遍历：通过 for..in..可以遍历字符串、列表、元组、字典、集合等可迭代对象

```python
#带下标的遍历--enumerate内置类的使用
nums = [23, 56, 5, 325, 78, 6, 7, 2]
for i, x in enumerate(nums):
    print(i, x)	#0 23/1 56/2 5/3 325/4 78/5 6/6 7/7 2
```

# 三、运算符

> **运算符的优先级和结合性**：
>
> ![](https://gitee.com/yao_zhimin/myimg/raw/master/20210108162409.png)

- 算术运算符

```python
# +加 -减 *乘 /除 **幂运算 //整除 %取余
print(1+1)	#2
print(4-2)	#2
print(3*2)	#6
print(7/2)	#3.5
print(7//2)	#3
print(3**3)	#27
print(81**0.5)	#9.0
print(10%3)	#1

#字符串中有限度的支持 '+' 和 '*'
print('hello'+'world')	# +将多个字符串拼接为一个字符串
print('hello'*3)	#	*用于将一个字符串重复多次
```

- 赋值运算符

```python
#'='等号在计算机编程中成为赋值运算符
#作用为将等号右边的值赋值给等号的左边(不能是常量或表达式)
x = 1
x += 1	#x = x+1	python中没有自增++和自减--运算
x -= 1	#x = x-1
x *= 2	#x = x*2
x /= 2	#x = x/2
x //= 2	#x = x//2 向下取整
x **= 2	#x = x**2

m,n = 3,5	#拆包（拆包时，变量个数和值的个数应一致）
print(m,n)

x = 'hello','good','yes'
print(x)	#x对应一个元组

o,*p,q = 1,2,3,4,5,6	#o = 1,p = [2,3,4,5],q = 6	*表示可变长度
```

- 比较运算符

```python
# 大于(>)、小于(<)、大于等于(>=)、小于等于(<=)、不等于(!=)、等等与(==)
print(2>1)	#True
print(2<4)	#True
print(4>=3)	#True
print(4<=9)	#True
print(5!=6)	#True
print('hello'=='hello')	#True

#在字符串里的使用
#字符串之间使用比较运算符，会根据各个字符的编码(ASCII)值逐一进行比较
print('a'>'b')	#False
print('abc'>'b')	#False
#数字和字符串之间进行==运算结果为False，进行!=运算，结果是True,不支持其他的比较运算
print('a' == 90)	#False
print('a' != 97)	#True
```

- 逻辑运算符

```python
#逻辑与(and)、逻辑或(or)、逻辑非(not)
#逻辑与(and):只要有一个运算数是False，结果就是False;只有所有的运算数都是True，结果才是True
print(2>1 and 5>3 and 10>2)	#True
print(3>2 and 5<4 and 6>1)	#False
#逻辑或(or):只要有一个运算数是True，结果就是True;只有所有的运算数都是Fasle，结果才是False
print(3>9 or 5>3 or 10<3)	#True
print(3<2 or 5<4 or 6<1)	#False
#逻辑非(not):取反，True==>False,False==>True

#逻辑运算的短路问题
4>3 and print('hello world')
4<3 and print('你好世界')	#逻辑与(and)运算的短路
#output:hello world

4>3 or print('hello world')
4<3 or print('你好世界')	#逻辑或(or)运算的短路
#output:你好世界

#逻辑运算的结果不一定是布尔值
#逻辑与(and)运算做取值时，取第一个为False的值；如果所有的运算数都是True，取最后一个值
print(3 and 5 and 0 and 'hello')	#0
#逻辑或(or)运算做取值时，取第一个为True的值；如果所有的运算数都是False，取最后一个值
print(0 or [] or 'lisi' or 5 or'ok')	#0
```

- 位运算符

```python
#按位与(&)、按位或(|)、按位异或(^)、按位左移(<<)、按位右移(>>)、按位取反(~)
a = 23	#0b0001 0111
b = 15	#0b0000 1111
#按位与(&):同为1才为1，否则为0
print(a&b)	#7	0b0000 0111
#按位或(|):有1则为1，否则为0
print(a|b)	#31	0b0001 1111
#按位异或(^):相同为0，不同为1
print(a^b)	#24	0b0001 1000
#按位左移(<<):a<<n ==> a*(2**n)
x = 5	#0b101
print(x << 2)	#20	0b10100
#按位右移(>>):a>>n ==> a//(2**n)
x = 15	#0b1111
print(x << 2)	#3	0b11
```

- 成员运算符

```python
#in 和 not in 运算符--用来判断一个内容在可迭代对象里是否存在
str = 'hello'
x = 'l'
y = 'z'
print(x in str)  # True
print(y not in str)  # True
```

# 四、条件判断语句和循环语句

## 1.条件判断语句

> python 中没有 switch 语句

- if 语句

```python
age = int(input('请输入你的年龄:'))
if age < 18:
    print('未满18岁，禁止进入')
```

- if...else...语句

```python
age = int(input('请输入你的年龄:'))
if age < 18:
    print('未满18岁，禁止进入')
else:
    print('Welcome 欢迎光临!')
```

- if...elif...elif...else 语句

```python
score = int(intput('请输入您的成绩:'))
if age >= 90:
    print('A')
elif age >= 80:
    print('B')
elif age >= 70:
    print('C')
elif age >= 60:
    print('D')
else:
    print('F')


#pass关键字在python里没有意义，单纯用来站位，保证语句的完整性
age = int(input('请输入您的年龄:'))
if age > 18:
    pass	#使用pass站位，保证程序完整性
else:
    print('您的年龄小于18岁')
#区间判断
score = int(intput('请输入您的成绩:'))
if 0<=score<60:
    print('你个辣鸡')
#隐式类型转换
if 4:	#if后面需要的是一个bool类型的值，如果不是布尔类型，会自动转换为布尔类型
    print('hello world!')
#三元表达式
num1 = int(input('请输入num1:'))
num2 = int(input('请输入num2:'))
print(num1 if num1 > num2 else num2)	#求两数中的较大数
```

## 2.循环语句

- while 语句

```python
sum = 0
count = 1
while count <= 100:
    sum += count
    count += 1
```

- for...in...语句

```python
#格式: for ele in iterable(可迭代对象--字符串、列表、字典、元组、集合、range...)
#range为内置类，用于生成指定区间的整数序列(顾头不顾尾)
for i in range(1,11):
    print(i)
for j in 'hello':
    print(j)
```

- for...else...语句

```python
#for...else...语句：当循环里的break没有被执行的时候，就会执行else
for i in range(101,201):
    for j in range(2,i):
        if(i%j==0):
            break
    else:
        print(i,'是素数')
```

- break 和 continue 语句

```python
#break和continue在python中只能用在循环语句中
#continue用于结束本轮循环，开始下一轮循环
#break用于结束循环
i = 0
while i<10:
    if i == 6:
        i+=1
        continue
    print(i)
    i+=1
#output:0、1、2、3、4、5、7、8、9
i = 0
while i<10:
    if i == 6:
        i+=1
        break
    print(i)
    i+=1
#output:0、1、2、3、4、5
```

# 五、函数

## 1.内置函数

- print 语句

```python
#def print(*values: object, sep: Optional[Text]=..., end: Optional[Text]=..., file: Optional[_Writer]=..., flush: bool=...)
#sep参数--用来表示输出时，每个值之间使用哪种字符作为分隔。默认使用空格作为分隔符
#end参数--当执行完一个print语句后接下来要输出的字符。默认为换行
print('hello','good','yes','hi',sep='+',end='----')
print('over')
#output:hello+good+yes+hi----over
```

- input 语句

```python
#def input(prompt: Any=...)
#input()  ==>括号里写提示信息
password = input('请输入您的银行卡密码:')
#不管用户输入的是什么，变量保存的结果都是字符串
```

- 进制转换

```python
#将int类型以不同的进制表现出来
a = 12
print(bin(a))	#0b1100
print(oct(a))	#0o14
print(hex(a))	#0xc
```

- 数据类型转换

```python
#将一个类型的数据转换为其他类型的数据
age = int(input('请输入您的年龄:'))	#input接收到的用户输入都是str字符串类型
#使用int内置类将数据转换为整数
a = '31'	#<class 'str'>
b = int(a)	#<class 'int'>

#如果字符串不是一个合法的数字，会直接报错
#x = 'hello'
#y = int(x)
#print(y)

x = '1a2c'
y = int(x,16)	#把字符串1a2c当做十六进制转换为整数
print(y)	#6700	默认使用十进制输出
print(bin(y))	#以二进制形式输出

m = '12'
n = int(m,8)	#把字符串的12当做八进制转换为整数
print(n)	#10

#使用内置float类可以将其他类型数据转换为float浮点数
a = '12.34'
b = float(a)
print(b + 1)

#如果字符不能转换成为有效的浮点数，会报错
#c = float('hello')
#print(c)

#将整形数据转换为字符串
c = 101
print(float(c))	#101.0

#使用str内置类可以将其他类型的数据装换为字符串
a = 34
b = str(a)
print(a)	#34
print(b)	#34
print(a+1)	#35
#print(b+1)	#报错

#使用bool内置类将其他类型数据转换为布尔值
#数字里，只有数字0被转换为布尔值是False，其他数字转换成为布尔值都是True
print(bool(100))	#True
print(bool(-1))	#True
print(bool(0))	#False
#字符串里，只有空字符串转换成为字符串型为False，其他都为True
print(bool('hello'))	#True
print(bool('False'))	#True
print(bool(''))	#False
#None转换为布尔值是False
print(bool(None))
#空list==>False、空元组==>False、空字典==>False、空集合==>False
print(bool([]))	#False
print(bool(()))	#False
print(bool({}))	#False
print(bool(set()))	#False
```

- 执行字符串里的代码--evla 函数

```python
a = 'input("请输入您的用户名:")'
eval(a)
```

> **执行结果：**![](https://gitee.com/yao_zhimin/myimg/raw/master/20210109163805.png)

- JSON 的使用

> |   python   |            |   JSON   |
> | :--------: | :--------: | :------: |
> |   字符串   |   ----->   |  字符串  |
> |    字典    |   ----->   |   对象   |
> | 列表、元组 |   ----->   |   数组   |
> |    True    |   ----->   |   true   |
> |   False    |   ----->   |  false   |
> |  **列表**  | **<-----** | **数组** |

```python
import json
person = {'name':'zhangsan','age':18,'gender':'female'}
m = json.dumps(person)	#dumps把列表、元组、字典等转换为JSON字符串
print(m)	#{"name": "zhangsan", "age": 18, "gender": "female"}
print(type(m))	#<class 'str'>
n = '{"name": "zhangsan", "age": 18, "gender": "female"}'
s = json.loads(n)	#loads可以将json字符串转换成为python里的数据
print(s)	#{'name': 'zhangsan', 'age': 18, 'gender': 'female'}
print(type(s))	#<class 'dict'>
```

- globals() 和 locals() 方法

```python
word = '你好'
def test():
    # 使用global对变量进行声明，可以通过函数修改全局变量的值
    global word
    word = 'hi'
    print(f"locals = {locals()},globals = {globals()}")
test()
#locals = {},globals = {'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x000002352A903970>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, '__file__': 'd:\\Code\\py\\test.py', '__cached__': None, 'word': 'hi', 'test': <function test at 0x000002352A907F70>}
```

+ sort方法

```python
students = [
    {'name': 'zhangsan', 'age': 18, 'score': 98, 'height': 180},
    {'name': 'lisi', 'age': 20, 'score': 95, 'height': 181},
    {'name': 'wangwu', 'age': 19, 'score': 90, 'height': 182},
    {'name': 'zhaoliu', 'age': 22, 'score': 91, 'height': 183},
]
# 字典和字典之间不能使用比较运算
# sort方法需要传递参数 key 指定比较规则，key 参数类型是函数
students.sort(key=lambda ele: ele['score'])  # 按照分数排序
print(students)
```

+ filter内置类

```python
# filter内置类--对可迭代对象进行过滤，得到的是一个filter对象
# filter可以给定两个参数，第一个参数是函数，第二个参数是可迭代对象
# filter结果是一个filter类型的对象，也是一个可迭代对象
ages = [12, 23, 34, 54, 30, 19, 22]
x = list(filter(lambda ele: ele > 18, ages))
print(x)  # [23, 34, 54, 30, 19, 22]
```

+ map内置类

```python
#map内置类--对列表内每个元素都执行一些相同的操作
ages = [12, 23, 34, 54, 30, 19, 22]
m = list(map(lambda ele: ele + 2, ages))
print(m)  # [14, 25, 36, 56, 32, 21, 24]
```

+ reduce内置类

```python
#作用为减项求和
from functools import reduce
scores = [100, 89, 76, 87]
print(reduce(lambda ele1, ele2: ele1+ele2, scores))  # 352

students = [
    {'name': 'zhangsan', 'age': 18, 'score': 98, 'height': 180},
    {'name': 'lisi', 'age': 20, 'score': 95, 'height': 181},
    {'name': 'wangwu', 'age': 19, 'score': 90, 'height': 182},
    {'name': 'zhaoliu', 'age': 22, 'score': 91, 'height': 183},
]
def bar(x, y):
    return x+y['age']
print(reduce(bar, students, 0))  # 79,最后一个参数0是对x的初始化
```

+ all,any方法

```python
#all作用为将可迭代对象的所有元素都转换为布尔值，若其中有False，则结果是False
#any方法与all结果相反，只要有一个是True，结果就是True
print(all(['hello', 'good']))  # True
print(all(['good', '']))  # False
```

+ dir方法

```python
#列出对象所有的属性和方法
nums = [1, 2, 3]
print(dir(nums))  # ['__add__', '__class__', '__class_getitem__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
```

+ 数学相关

```python
#abs方法--取绝对值
print(abs(-9))  # 9
#divmod方法--求两数相除的商和余数
a, b = divmod(5, 2)
print(a, b)  # 2 1
#max--求最大数
#min--求最小数
#pow--求幂运算 等同于 **
#round--四舍五入保留到指定小数位
#sum--求和
```

+ exit方法

```python
#以指定的退出码退出程序
print(abs(-9))  # 9
exit(100)
```

> ![](https://gitee.com/yao_zhimin/myimg/raw/master/20210113201203.png)

+ help方法

```python
#查看对象的帮助文档
print(help(int))
```

> <img src="https://gitee.com/yao_zhimin/myimg/raw/master/20210113201409.png" style="zoom: 67%;" />

+ id--获取一个数据的内存地址

```python
a = 12
print(f"id(a) = {id(a)}")  # id(a) = 1575615949456
```

+ 判断对象相关

```python
#isinstance--判断一个对象是否是由一个类创建出来的
#issubclass--判断一个类是否是另一个类的子类
```

+ open--用来打开一个文件
+ repr--将一个对象转换为字符串

## 2.自定义函数

> 1. 函数的三要素：函数名、参数、返回值
> 2. 在python里，不允许函数重载，如果重名，后一个函数会覆盖前一个函数
> 3. python里，函数名也可以理解为一个变量名

- 基本用法

```python
#	def func(形参):
#		doing something
#	func(实参)	#通过函数名调用函数
```

- 函数的参数

```python
#形参的值是不确定的，只是用来站位的
#函数调用时传入的参数，才是真正参与运算的数据，我们称之为实参

#默认参数的使用
def say_hello(name='Jack', age=18):
    print(f"大家好，我是{name},我今年{age}岁了")
say_hello()  # 大家好，我是Jack,我今年18岁了
say_hello('Tony')   # 大家好，我是Tony,我今年18岁了

#缺省参数的使用:传了参数就用传的参数，否则使用默认参数
say_hello(age=20)   # 大家好，我是Jack,我今年20岁了

#可变参数的使用
def add(a, b, *args, mul=1, ** kwargs):  # a,b为未知参数,*args表示可变位置参数,mul为缺省参数,**kwargs为关键字参数
    print(f"a={a},b={b}")  # 多出来的可变位置参数会以元组的形式保存到args里
    print(f"args={args}")
    print(f"kwargs={kwargs}")  # 多出来的关键字参数会以字典的形式保存
    c = a+b
    for arg in args:
        c += arg
    return c * mul
print(add(1, 3, x=1, y='g'))
print("-------------")
print(add(3, 4, 5, 6, 7, 6, mul=2))
```

>  ![](https://gitee.com/yao_zhimin/myimg/raw/master/20210113180034.png)

- 函数的返回值

```python
#返回值就是函数执行的结果，，并不是所有的函数都必须要有返回值
#一般情况下，一个函数最多只执行一个return语句；特殊情况下(finally语句)，一个函数会执行多个return语句
def add(a, b):
    return a+b
print(add(1, 2)**4) # 81

#如果一个函数没有返回值，它的返回值就是None

#多个返回值
def test(a, b):
    x = a//b
    y = a % b
    # return[x,y]	返回一个列表
    # return{'x':x,'y':y}	返回一个字典
    return x, y  # 一般用这个，就是一个元组
result = test(3, 5)
print(f"商是{result[0]},余数是{result[1]}") #商是0,余数是3
```

- 全局变量和局部变量

```python
#在python中，只有函数能够分隔作用域
word = '你好'	#全局变量
def test():
    a = 12	#局部变量，只能在函数内部使用
    # 使用global对变量进行声明，可以通过函数修改全局变量的值
    global word
    word = 'hi'
test()
print(word)  # hi
```

+ 可变类型和不可变类型

```python
def test(a):
    a = 100
def demo(nums):
    nums[0] = 10
x = 1
test(x)
print(x)  # 1
y = [3, 5, 6, 8, 2]
demo(y)
print(y)  # [10, 5, 6, 8, 2]
```

+ 递归函数的使用

```python
# 简单来说，递归就是函数内部调用自己
# 递归最重要的就是找到出口(停止的条件)
# break语句只能用于循环语句，不能用于递归
def get_sum(n):
    if n == 0:
        return 0
    return get_sum(n-1) + n
print(get_sum(10))  # 55
```

+ 匿名函数的使用

```python
# 除了使用 def 关键字定义一个函数以外，我们还能用 lambda 表达式定义一个函数
# 匿名函数，用来表达一个简单的函数，函数调用的次数很少，基本上就是调用一次
# 调用匿名函数的两种形式：
# 1.给它定义一个名字(很少这样使用)
def mul(a, b): return a + b
print(mul(1, 2))  # 3
# 2.把这个函数当作一个参数传给另一个函数使用(使用场景较多)
def calc(a, b, fn):
    return fn(a, b)
x1 = calc(1, 2, lambda x, y: x+y)
x2 = calc(2, 3, lambda x, y: x-y)
x3 = calc(1, 2, lambda x, y: x*y)
x4 = calc(2, 3, lambda x, y: x/y)
print(x1, x2, x3, x4)   # 3 -1 2 0.6666666666666666
```

+ 高阶函数

  1. 一个函数作为另一个函数的返回值
  2. 一个函数作为另一个函数的参数
  3. 函数内部再定义一个函数

  ```python
  #闭包：如果在一个内部函数里，对在外部作用域（但不是全局作用域）的变量进行引用，那么内部函数就被认为是闭包
  def outer():
      x = 10
      def inner():
          nonlocal x  # 在内部函数修改外部函数的局部变量
          y = x + 1
          print(f'y in inner = {y}')
          x = 20
      return inner
  outer()()  # y in inner = 11
  
  #计算一段代码的执行时间
  
  ```

  

# 六、模块操作

> 模块：在python里一个py文件，就可以理解为一个模块
>
> 不是所有的py文件都能作为一个模块来导入
>
> 如果想让一个py文件能够被导入，模块名字必须要遵守命名规则

## 1.内置模块的使用

+ 导入模块

```python
import time	# 使用 import 模块名直接导入一个模块
print(time.time())

from random import randint	# from 模块名 import 函数名，导入一个模块里的方法
randint(0,2)

from math import *	# from 模块名 import *，导入一个模块里的"所有"方法和变量
print(pi)

import datetime as dt	# 导入一个模块并给这个模块起一个别名
print(dt.MAXYEAR)

from copy import deepcopy as dp	# from 模块名 import 函数名 as 别名
dp(['hello', 'good'.'hi'], [1, 2, 3], 'hi')
```

+ 常见的内置模块

```python
# os模块(OperationSystem)--用来调用操作系统里的方法
import os
# os.name--获取操作系统的名字   Windows==>nt  非Windows==>posix
print(os.name)  # nt
# os.sep--路径的分隔符  Windows==>\  非Windows==>/
print(os.sep)  # \
# os.path--文件路径相关方法
print(os.path.abspath('test.py'))  # 获取文件的绝对路径 d:\Code\py\test.py
print(os.path.isdir('py'))  # 判断是否是文件夹
print(os.path.isfile('py'))     # 判断是否是文件
print(os.path.exists('test.py'))    # 判断是否存在
filename = '2021.01.13.demo.py'
print(os.path.splitext(filename))  # ('2021.01.13.demo', '.py')
print(os.getcwd())  # 拿到文件所在目录  d:\Code\py


# random模块--用来生成一个随机数
import random
# randint(a, b)--用来生成[a,b]的随机整数
print(random.randint(2, 9))
# random()--用来生成[0,1)的随机浮点数
print(random.random())
# randrange(a, b)--用来生成[a,b)的随机整数
print(random.randrange(2, 9))
# choice()--用来在可迭代对象里随机抽取一个数据
print(random.choice(['zhangsan', 'lisi', 'jack', 'tony']))
# sample(iter,n)--用来在可迭代对象里随机抽取n个数据
print(random.sample(['zhangsan', 'lisi', 'jack', 'tony'], 2))
```

## 2.第三方模块的使用

+ 下载第三方模块

> 通过 pip install 模块名 可以下载第三方模块

+ 查看已下载的第三方模块

> pip list

+ 使用第三方模块

> import 第三方模块名

+ 卸载第三方模块

> pip uninstall 模块名 

## 3.使用自定义模块

> 如果一个py文件想要当做一个模块被导入，文件名一定要遵守命名规范
>
> 导入一个模块，就能使用这个模块里的变量和方法了

# 七、文件操作

+ 打开和关闭文件

```python
# python里使用 open 内置函数打开并操作一个文件
# open 参数介绍
# file: 用来指定打开的文件(不是文件名，而是文件的路径)
# mode: 打开文件的模式，默认是 r(只读)
# encoding: 打开文件时的编码方式，默认为gbk(windows)

# open 函数会有一个返回值，表示打开的文件对象
file = open('xxx.txt', encoding="utf-8")
print(file.read())  # 今天天气好晴朗(xxx.txt文件中的内容)
file.close()    # 操作完成，关闭文件
```

+ 文件路径详解

```python
#1.绝对路径--从电脑盘符开始的路径
# 第一种方式
file = open('D:\\Code\\py\\xxx.txt', encoding="utf-8")
# 第二种方式
file = open(r'D:\Code\py\xxx.txt', encoding="utf-8")
# 第三种方式--推荐使用
file = open('D:/Code/py/xxx.txt', encoding="utf-8")

#2.相对路径(用的更多)--当前文件所在的文件夹开始的路径
# ./ 可以省略不写，表示当前文件夹
file = open('xxx.txt', encoding="utf-8")
# ../ 回到上一级
file = open('../xxx.txt', encoding="utf-8")
```

+ 文件的打开方式详解

```python
# mode---文件的打开方式
# r(默认方式):只读，打开文件后，只能读取，不能写入；如果文件不存在，会报错
# w:只写，打开文件后，只能写入，不能读取。如果文件存在，会覆盖；如果文件不存在，会创建新文件
# b:以二进制形式打开文件    rb:以二进制读取  wb:以二进制写入
# a:追加模式，会在文件最后追加内容。如果文件不存在，会创建文件；如果文件存在，会追加
# r+:可读可写   w+:可写可读 (用的很少)
```

+ csv文件读写

```python
import csv
file = open('demo.csv', 'w', encoding="utf8", newline='')
# 写文件
wr = csv.writer(file)
wr.writerow(['name', 'age', 'score', 'city'])
wr.writerow(['zhangsan', '19', '90', 'Beijing'])
wr.writerow(['lisi', '19', '90', 'NewYork'])
file.close()

# 读文件
file = open('aaa.csv', 'r', encoding="utf8", newline='')
re = csv.reader(file)
for data in re:
    print(data)
file.close()
"""
['姓名', '年龄', '性别', '成绩']
['zhangsan', '23', 'male', '90']
['lisi', '30', 'female', '89']
['wangwu', '30', 'male', '67']
"""
```

+ 序列化和反序列化

```python
#序列化：将数据从内存持久化保存到硬盘的过程
#反序列化：将数据从硬盘加载到内存的过程

# json(只能保存一部分信息,作用是用来在不同的平台里传递数据)--本质就是字符串，区别在于 json 里要用双引号表示字符串
# json 里将数据持久化有两个方法
# 1.dumps--将数据装换成为字符串，不会将数据保存到文件里。
names = ['zhangsan', 'lisi', 'jack', 'tony']
x = json.dumps(names)  #
print(x)  # ["zhangsan", "lisi", "jack", "tony"]
file = open('names.txt', 'w', encoding='utf8')
file.write(x)
file.close()
# 2.dump--将数据装换成为字符串的同时写入到指定文件
names = ['zhangsan', 'lisi', 'jack', 'tony']
file = open('names.txt', 'w', encoding='utf8')
json.dump(names, file)
file.close()
# json 反序列化也有两个方法
# 1.loads--将json字符串加载为python里的数据
x = '{"name":"zhangsan","age":18}'
p = json.loads(x)
print(p['name'])  # zhangsan
# 2.load--读取文件，把读取的内容加载成为python里的数据
file = open('names.txt', 'r', encoding="utf8")
y = json.load(file)
print(y[0])  # zhangsan
file.close()

# pickle(用来将数据原封不动的转换成为二进制，但是这个二进制，只能在python里识别)--将python里任意的对象转换成为二进制
import pickle
# 序列化:
# dumps--将python数据转换成为二进制
names = ['zhangsan', 'lisi', 'jack', 'henry']
b_names = pickle.dumps(names)
file = open('names.txt', 'wb')
file.write(b_names)  # 写入的是二进制，不是存文本
file.close()
# dump--将python数据转换成二进制，同时保存到指定文件
names = ['zhangsan', 'lisi', 'jack', 'henry']
file2 = open('names.txt', 'wb')
pickle.dump(names, file2)
# 反序列：
# loads--将二进制加载成为python数据
file1 = open('names.txt', 'rb')
x = file1.read()
y = pickle.loads(x)
print(y)  # ['zhangsan', 'lisi', 'jack', 'henry']
file1.close()
# load--读取文件，并将文件的二进制内容加载为python数据
file3 = open('names.txt', 'rb')
x = pickle.load(file3)
print(x)  # ['zhangsan', 'lisi', 'jack', 'henry']
```



# 八、面向对象部分

