[TOC]

# Python基础

## 01_数据类型

### 1.1 六种基本数据类型

> 前三种可变 后三种不可变

1. Number（数字）
   + int
   + float
   + bool
   + complex(复数)
2. String（字符串）
3. Tuple（元组）
4. List（列表）
5. Set（集合）
6. Dictionary（字典）

### 1.2强制类型转化

int(a)  float(a)  String(a)

```python
# python输入测试
# input()   input获得的信息都是字符串

b = input('用户提示信息:')
print("我是输入信息%s" % b)
print('b的类型是', type(b))
print('-------------')
print('b的类型是:', type(int(b)))
print('-----------')
a = float(b)
print('b的类型是:' , type(a))
```

## 02_辨别数据类型

### 2.1 type()

不会认为子类是一种父类类型。

### 2.2 isinstance()

会认为子类是一种父类类型。

### 2.3语法

type(a) == int

type()

isinstance(a,int)

```python
a = 'a'
b = "bb"
c = 2
d = True
print(isinstance(a, bool))
print(isinstance(b, str))
print(isinstance(c, str))
print(isinstance(d, bool))
print(isinstance(d, str))
print('-------------')
print(type(a))
print(type(b))
print(type(c))
print(type(d))


​```
False
True
False
True
False
-------------
<class 'str'>
<class 'str'>
<class 'int'>
<class 'bool'>
​```
```



## 03_PEP8规范

1. 单行注释后加空格 `# lalala`
2. 代码最后一行要是空行
3. 行内注释需要两个空格

## 04_输入输出

### 4.1输出

类似于c语言  %s:string占位符   %d:十进制整数占位符   %s:小数浮点数输出   \n换行

```python
# python输出测试
# int string float
print(111)
print(1.111)
print('你好世界')

# expression
a = 1
b = 2
c = 'lalal'
print(a)
print(a, b)  # a和b之间默认空格隔开
print(a, b, c)  # 三者之间空格隔开

# %s %d %f
aa = 1
bb = 2
cc = '你好世界'
dd = 12345.12345
print('我是数字%d' % aa)
print('我是数字%d+%d的和%d' % (aa, bb, aa+bb))
print('我是python的第一行%s' % cc)
print('%.3f  想保留几位小数点后就写几' % dd)

# 默认print输出后会换行
# 使用end=" "
print("使用end可以不换行,end后的字符串就是两者之间隔的", end=" ")
print("看你换了没")

# 字符串中想换行  \n
print("看你换\n了没")
```

### 4.2输入

格式input("用户提示信息")

值得一提的是 input获取的信息都是字符串类型

```python
# python输入测试
# input()   input获得的信息都是字符串

a = input()
print("input里可以不写提示信息,但是非常不友好,%s" % a)
print("-----------")
b = input('用户提示信息:')
print("我是输入信息%s" % b)
```

## 05_字符串操作相关

### 5.1 字符串输出切片等

1. [start: end: step]   不包含end
2. [start: ]   从index=start开始输出
3. [start: end: step]     stop<0则是倒序  值得一提的是 此时end需要>start
4. [: end]     从index=0开始  值得一提 如果end<0 则是倒数第一个(不包含)   
5. ........自己探索更多好玩

### 5.2查找find(),rfind()

> find从左找
>
> rfind()从右找

1. find方法 查找字符串
2. my_str.(queryStr, start, end)
3. queryStr 需要在find中查找的字符串    start开始的index end结束的index
4. start和end可以省略 省略的攻略自己找(可以省略一个或者多个)
5. 找到返回index值   未找到返回-1

```python
# find方法 查找字符串
# my_str.(queryStr, start, end)
# queryStr 需要在find中查找的字符串    start开始的index end结束的index
# 找到返回index值   未找到返回-1
a = 'lala lalala wo shi mai bao de xiaohangjia'
b = a.find("mai", 0, len(a))
print(b)
```

### 5.2 统计出现的次数

```python
str.count(sub, start= 0,end=len(string))
```

### 5.3 replace()替换

1. replace 替换
2. my_str.replace(str1, str2, count=my_str.count(str1))
3. my_str原始字符串,str1是原始字符串中存在的字符串,str2 需要替换后的字符串,count是替换多少个默认是my_str.count(str1)即全部替换

```python
# replace 替换
# my_str.replace(str1, str2, count=my_str.count(str1))
# my_str原始字符串,str1是原始字符串中存在的字符串,str2 需要替换后的字符串,count是替换多少个默认是my_str.count(str1)即全部替换
a = 'lalala lalala wo sho mai bao de xiaohangjia'
b = a.replace("la", "hh", a.count("la")-1)
print(b)
```

### 5.4 split(),rsplit()分割字符串

> 一个从前一个从后

1. split()分割
2. str.split(str="", num=string.count(str))
3. str是以str分隔 num分割几次  默认全部
4. 返回一个列表(数组)

```python
# split()分割
# str.split(str="", num=string.count(str))
# str是以str分隔 num分割几次  默认全部
# 返回一个列表(数组)
a = 'lalala lalala wo sho mai bao de xiaohangjia'
b =  a.split(" ")
print(b)
```

### 5.5 join()连接方法

1. str.join(str1)
2. 用str连接str1
3. 值得一提 if str1是列表则连接列表的一个元素   elif str1是字符串则连接字符串中每一个字符

```python
# str.join(sequence)
# 用str连接sequence
a = "_"
b = ('lala', 'hhh', 'hehe')
c = ('lala')
d = 'hhhhhh'

print(a.join(b))
print(a.join(c))
print(a.join(d))
```

## 06_List列表

格式:  list1 = []

list2 = [1,2,'lala',True,4.14]

1. 新建列表
2. 遍历列表
3. 插入数据
4. 删除数据
5. 排序和倒序

```python
# List列表
a = []
b = [1, 3.14, True, 'lalala']
b[2] = 3.14159
print(a, type(a), len(a))
print(b, type(b), len(b))
print('-------------')
# 遍历
for c in b:
    print(c, end=" ")
print('-------------')

# 添加新元素方法
# append()
# insert(index, data)
# extend(可迭代对象)    如果是字符串将字符串的,每一个字符添加末尾,如果是列表就按元素添加
b.append('hhhh')
print(b)
b.insert(2, 'qqqq')
print(b)
b.extend("hello")
print(b)
b.extend([1, 2,"lalal"])
print(b)
print('-------------')

# 删除元素
# del()  根据index删除
# pop(index)  默认删除最后一个element
# remove()  根据element.val删除
aa = [1, 2, 3, 4, 5, 6, 7, 8, 9]
aa.remove(4)
print(aa)
aa.pop()
print(aa)
aa.pop(1)
print(aa)
del aa[3]
print(aa)
print('-------------')

# 列表的排序

# 列表.sort()   升序
# 列表.sort(reverse=True) 倒序
a = [3, 87,  432, 76, 4.14]
a.sort()
print(a)
a.sort(reverse=True)
print(a)

# sorted()排序  会生成一个新列表 原列表不变
aa = [3, 87,  432, 76, 4.14]
print(aa)
print(sorted(aa))
# 倒序
print(sorted(aa, reverse = True))

# 逆置
bb = [3, 87,  432, 76, 4.14]
print(bb[::-1])
bb.reverse()
print(bb)

```

## 07_tuple元组

> 元组和列表类似,但是元组的element不能修改,只能用于查找.
>
> 列表使用方括号,元组使用圆括号

```python
# 元组
# 元组和列表类似,但是元组的element不能修改,只能用于查找.
# 列表使用方括号,元组使用圆括号
a = (1)
print(a, type(a))  # 1 <class 'int'>一个元素的元组不会被认为是元组

aa = (1,)
print(aa, type(aa))  # (1,) <class 'tuple'>如果只有一个元素的元组需要使用逗号

# tuple.index(val,start,end) 查找值为val的元素的index
aaa = (1, 2, 3, 45, 2, 1, 3, 1, 3)
print(aaa.index(2))

# tuple.count(val)   查询值=val的元素个数
bbb = (1, 2, 3, 45, 2, 1, 3, 1, 3)
print(bbb.count(1))
```

## 08_字典dict

1. 类似于java的map  由key和value组成
2. 其中key必须是不可变类型(int float bool)
3. value可以是可变类型
4. dict[key]  可以返回value  如果key不存在会报错
5. dict.get(key) 可以返回value  如果key不存在返回none
6. 增加
7. 修改
8. 删除

```python
# 字典dict
a = {"name": "zhangsan", "sex": "男", 'like': ['上学', '上网', '购物']}

print(a, type(a))
print('----------')

# 添加和修改
# dict[key] = value
# if(exist(key)) {update dict} elif(!exist(key)){add key and value}
a["born"] = "xian"
print(a)
a["born"] = "beijing"
print(a)
print('----------')

# 删除数据
# del
del a["sex"]
print(a)
# dict.pop(val)   返回被删除key的value
result = a.pop("born")
print(a)
print(result)
# clear() 清空字典
a.clear()
print(a)
# del 字典名    删除这个字典(后面也不能使用这个变量了)
del a
print(a)       # 报错

# 根据key查找value
# dict[key]  可以返回value  如果key不存在会报错
# dict.get(key) 可以返回value  如果key不存在返回none
# dict.get(key, value) 可以返回value  如果key不存则返回书写的value
print(a["sex"])
# print(a["sex1"])    # 报错
print(a.get('like')[1])
print(a.get('born', "xian"))
print(a)

# 遍历
for key in a:
    print(key, a[key])

# dict.keys()  返回key的列表 其实是dict_keys类型的列表  可以强制转化为list
print(a.keys())  # dict_keys(['name', 'sex', 'like'])
print(type(a.keys()))  # dict_keys

# dict.value()  返回所有的value列表 其实的dict_value类型的列表  可以强制转化为list
print(a.values())
print(type(a.values()))


# 字典.item()  获取所有的键值对 类型是dict_items  由key,value组成的元组
print(a.items())  # dict_items([('name', 'zhangsan'), ('sex', '男'), ('like', ['上学', '上网', '购物'])])
print(type(a.items()))  # <class 'dict_items'>
# 强制转元组试试
print(tuple(a.items()))  # (('name', 'zhangsan'), ('sex', '男'), ('like', ['上学', '上网', '购物']))
print(type(tuple(a.items())))  # <class 'tuple'>
print('----------')

# 遍历items
for k, v in a.items():  # k是元组中第一个数据  v是元组中第二个数据
    print(k, end=": ")
    print(v)

```

## 09_判断

### 9.1 if ..elif.. else

1. if后有一个空格
2. 判断条件后有冒号
3. else和if相同缩进

```python
boardIn = int(input('请输入年龄:'))
if boardIn >= 18 & boardIn > 200:
    print('你18岁了')
elif boardIn < 0:
    print('你还没出生')
else:
    print('好家伙你个老不死的')
print('好了不闹了结束了')
```

## 10_循环

### 10.1 while

结果 1 + 2  = 3

​		1 + 2 + 3 = 6

​		........

```python
# 1+2+3+...+10
i = 1
a = ""
b = 0
while i < 11:
    a = a + " + " + str(i)
    b = b + i
    print('%s = %d' % (a[3:], b))
    i += 1
```

### 10.2 for循环

1. 遍历字符串

   ```python
   # for循环字符串
   for i in "HelloWorld":
       print(i)
   ```

2. 循环数

```python
# for循环数
# range(n) 会生成[0,1,2,3,4....n)  不包含n
for i in range(5):
    print('我是%d' % i)
print('-----------')
# range(a,b)   会生成[a,a+1,a+2,a+3....b)
for i in range(3, 7):
    print('我是%d' % i)
print('-----------')
# range(a,b,step)   会生成[a,a+step,a+step,a+step....b)
for i in range(3, 10, 2):
    print('我是%d' % i)
    
```

### 10.3 continue Break

1. break和continue是python的两个关键字
2. 这两个关键字只能在循环中使用
3. break是终止循环
4. continue是跳过本次循环,进入下一次循环
5. 一般是for + if 中使用

```python
# for循环数 跳过3 而且终止于7
for i in range(10):
    if i == 3 :
        continue
    elif i == 7:
        break
    print(f'我是{i}')
```

### 10.4 else循环结构

1. 和for或者while同级缩进
2. 正常结束循环时会进入(!break)

```python
# "HELLO WORLD"包含o 输出包含 不包含输出不包含
for i in "HELLO WORLD":
    if i == 'O':
        print('包含')
        break
else:
    print('不包含')
```

```python
# "HELLO WORLD"包含o 输出包含 不包含输出不包含
for i in "HELLO WORLD":
    if i == 'O':
        print('包含')
        break
else:
    print('不包含')

# The second way
# len()返回字符串长度   a.replace(b,c) 在a中的b替换成c
a = 'HELLO WORLD'
b = 'O'
c = a.replace(b, "")
print(c)
if len(c) < len(a):
    print('包含')
else:
    print('不包含')
```

## 11_函数

### 11.1 定义

关键字 def 函数名():

```python
def print_info():
    print('你好世界');
```



### 11.2 调用

```python
print_info()
```



### 11.3 测试

```python
import datetime


def days(date):
    date = date.split('/')
    if len(date) != 3:
        return False
    else:
        year = int(date[0])
        month = int(date[1])
        day = int(date[2])
        return year, month, day  # 默认返回元组


def be_days(date0, date1):
    days1 = datetime.date(days(date0)[0], days(date0)[1], days(date0)[2])
    days2 = datetime.date(days(date1)[0], days(date1)[1], days(date1)[2])
    return abs(days2 - days1)


date0 = input('please input a little date:\n>')    
date1 = input('please input a big date:\n>')
print(be_days(date0, date1))
```

```python

a = 100   # 会运行
def add(aa, b):   # 不会运行
    print(aa + b + a)
add(10, 50)   # 会运行
print(a)    # 会运行
def setA():     # 不会运行
    a = 50
    print(a)
aaa = setA()     # 会运行
```





### 11.4 全局和局部变量

1. 全局:可以在函数外使用(函数外定义的变量就是全局变量)
2. 局部:只能在函数内部使用(函数内定义的变量就是局部变量)



### 11.5 return

1. return [a, b]
2. return (a, b)
3. return {'a': a, 'b': b}
4. return a, b    默认返回元组
5. return    默认返回null 用来终止函数运行
6. 不写return  默认也返回null  即就是函数至少返回一个null

### 11.6 函数嵌套

```python
# 函数嵌套
def a():
    print('我是a')
def b():
    print('我是b')
    a()
a()
b()
```

### 11.7函数传参

1. 实参传参
2. 关键字传参
3. 混合传参
4. 缺省参数
5. 不定长传参
6. 注意如果以上五个混合使用,形参要在最前
7. 顺序依次是 1->2->3->4->5

```python
# 函数传参
def func(a, b, c):
    print(a)
    print(b)
    print(c)

func(1, 2, 3)

print('-'*10)

# 关键字传参
func(a=5, c=10, b=20)

# 混合传参  注意位置实参应该放在关键字实参前  一个形参给两次值也会报错 
func(5, c=10, b=20)

# 缺省参数
def a(aa, b, c=10):
    print(aa)
    print(b)
    print(c)
a(1, 2, 3)
print('-'*10)
a(1, 5)

# 不定长传参
# 一个星可以接收不定长的元素作为元组存在
# 两个行可以接收不定长元素作为字典存在
def func(*a, **b):
    print(a)
    print(b)
func(1, 2, 3, 4, 5, 6)    # *a输出(1, 2, 3, 4, 5, 6)   **b为空{}
print('-' * 10)
func(a=1, b=2, c=3, d=4, f=5)     # *a为空   **b为{'a': 1, 'b': 2, 'c': 3, 'd': 4, 'f': 5}
print('-' * 10)
func(1, 2, 3, 4, a=1, b=2, c=3)   # *a为(1, 2, 3, 4)   **b为{'a': 1, 'b': 2, 'c': 3}

# 不定长传参测试
def func(*a, **b):
    num = 0
    for i in a:
        num += i
    print(num)
    for j in b.values():
        num1 = 0
        num1 += j
    print(num1)
func(1, 2, 3, 4, a=1, b=2, c=3)  # 10   3


```

## 12_组包和拆包

> 交换巧用



示例:

```python
# 组包 拆包

a = 1, 2, 3, 4, 5  # 组包
print(a)  # (1, 2, 3, 4, 5)
print(type(a))  # <class 'tuple'>
print('-' * 10)

def func():
    return 1, 2, 3  # 组包

aa = func()
print(aa)  # (1, 2, 3)
print(type(aa))  # <class 'tuple'>
print('-' * 10)

b, c, d, e, f = a  # 拆包(个数不匹配会报错)
print(b, c, d, e, f)  # 1 2 3 4 5
print('-' * 10)

g, h, i = func()
print(g)  # 1
print(h)  # 2
print(i)  # 3
print('-' * 10)

j, k = [1, 2]
print(j)
print(k)

# 交换ab
c = a
a = b
b = c

# 交换ab
a = a + b
b = a - b
a = a - b

# 交换ab
a, b = b, a  # 组包 拆包一起
```

## 13_引用和类型可变性

> 引用赋值传参: 传递的不是数值而是地址
>
> 不可变类型: 单个数值(int float bool str)和不可变多元素(元组tuple) 重新赋值时会新建一块内存地址,原来于其他变量产生的赋值操作不影响
>
> 不可变类型: 可变多元素(list,dict)重新赋值时会新建一块内存地址,原来于其他变量产生的赋值操作不影响
>
> 可变类型: 可变多元素(list,dict)对原来进行增删改查操作时仅仅在原地址更改,所以之前和其他变量有过赋值操作时也会影响其他变量的值

### 13.1 变量的引用

| 类型   | int  | float | boolean | str  | tuple | list | dict |
| ------ | ---- | ----- | ------- | ---- | ----- | ---- | ---- |
| 可变   |      |       |         |      |       | √    | √    |
| 不可变 | √    | √     | √       | √    | √     |      |      |

```python
# 引用
# 可变性和不可变性解释

print('-' * 10, 'int测试')   # 地址发生变化
a = 1
b = a
a = 2
print(f'a={a}', id(a))  # a=2 1431720913232
print(f'b={b}', id(b))  # b=1 1431720913200

print('-' * 10, 'float测试')  # 地址发生变化
a = 3.14
b = a
a = 2.73
print(f'a={a}', id(a))  # a=2.73 1431722761072
print(f'b={b}', id(b))  # b=3.14 1431723699472

print('-' * 10, 'boolean测试')  # 地址发生变化
a = True
b = a
a = False
print(f'a={a}', id(a))  # a=False 140732998514824
print(f'b={b}', id(b))  # b=True 140732998514792

print('-' * 10, 'String测试')  # 地址发生变化
a = '111'
b = a
a = '222'
print(f'a={a}', id(a))  # a=222 1773552782576
print(f'b={b}', id(b))  # b=111 1773552782384

print('-' * 10, 'tuple测试')  # 地址发生变化
a = (1, 2, 3, 4)
b = a
a = (1, 2, 3, 4, 5)
print(f'a={a}', id(a))  # a=(1, 2, 3, 4, 5) 2735467688400
print(f'b={b}', id(b))  # a=(1, 2, 3, 4, 5) 2735467688400

print('-' * 10, 'list重置测试')  # 地址发生变化
a = [1, 2, 3, 4]
b = a
a = [1, 2, 3, 4, 5]
print(f'a={a}', id(a))  # a=(1, 2, 3, 4, 5) 2735467688400
print(f'b={b}', id(b))  # b=[1, 2, 3, 4] 1578461787520

print('-' * 10, 'list新增测试')  # 地址没有发生变化
a = [1, 2, 3, 4]
b = a
a.append(5)
print(f'a={a}', id(a))  # a=[1, 2, 3, 4, 5] 1578461961280
print(f'b={b}', id(b))  # b=[1, 2, 3, 4, 5] 1578461961280

print('-' * 10, 'dict重置测试')  # 地址发生变化
a = {1: 11, 2: 22, 3: 33, 4: 44}
b = a
a = {1: 11, 2: 22, 3: 33, 4: 44, 5: 55}
print(f'a={a}', id(a))  # a={1: 11, 2: 22, 3: 33, 4: 44, 5: 55} 1898348908928
print(f'b={b}', id(b))  # b={1: 11, 2: 22, 3: 33, 4: 44} 1898348908864

print('-' * 10, 'dict新增测试')  # 地址没有发生变化
a = {1: 11, 2: 22, 3: 33, 4: 44}
b = a
a[5] = 55
print(f'a={a}', id(a))  # a={1: 11, 2: 22, 3: 33, 4: 44, 5: 55} 1898353441920
print(f'b={b}', id(b))  # b={1: 11, 2: 22, 3: 33, 4: 44, 5: 55} 1898353441920



```

### 13.2 函数传参的引用

> 函数传参也是用引用传参
>
> 规则同上述变量传参



## 14_示例 学生管理系统

```python
# 学生管理系统

import sys

# global dict
students = [{'name': 'aaa', 'sex': '男', 'age': '18'}]


# select Option
def selectOption():
    print("1. 添加学生")
    print("2. 删除学生")
    print("3. 修改学生信息")
    print("4. 查询单个学生信息")
    print("5. 查询所以学生信息")
    print("6. 退出系统")
    s = input("请输入想要操作的编号:")
    judgeForwarding(s)
    return


# 判断和转发请求
def judgeForwarding(optionNum):
    if optionNum == '1':
        insertStu()
    elif optionNum == '2':
        delStu()
    elif optionNum == '3':
        updateStu()
    elif optionNum == '4':
        queryStu()
    elif optionNum == '5':
        queryStus()
    elif optionNum == '6':
        sys.exit()
    return


# 添加学生
def insertStu():
    studentDict = {}
    print("请输入需要添加学生的信息:")
    name = input("name:")
    sex = input("sex:")
    age = input("age:")
    studentDict['name'] = name
    studentDict['sex'] = sex
    studentDict['age'] = age
    students.append({'name': name, 'sex': sex, 'age': age})
    print('添加成功')
    selectOption()
    return


# delete
def delStu():
    name = input("请输入需要删除学生的名字:")
    for i in students:
        if i['name'] == name:
            students.remove(i)
    print('删除成功')
    selectOption()
    return


# update
def updateStu():
    name = input("请输入需要修改学生的名字:")
    option = input('请输入您想修改的项目1. name  2. sex  3. age')

    for i in students:
        if i['name'] == name:
            if option == '1':
                s = input('请输入新名字:')
                i[name] = s
            elif option == '2':
                s = input('请输入新性别:')
                i['sex'] = s
            elif option == '2':
                s = input('请输入新年龄:')
                i['age'] = s
    print('修改成功')
    selectOption()
    return


# query student
def queryStu():
    name = input("请输入需要查询学生的名字:")
    for i in students:
        if i['name'] == name:
            print(i)
    selectOption()
    return


# query students
def queryStus():
    for i in students:
        print(i)
    selectOption()
    return


# start
selectOption()
```

## 15_递归

> 在函数内部，可以调用其他函数。如果一个函数在内部调用自身本身，这个函数就是递归函数。

递归函数特性：

1. 必须有一个明确的结束条件；
2. 每次进入更深一层递归时，问题规模相比上次递归都应有所减少
3. 相邻两次重复之间有紧密的联系，前一次要为后一次做准备（通常前一次的输出就作为后一次的输入）。
4. 递归效率不高，递归层次过多会导致栈溢出（在计算机中，函数调用是通过栈（stack）这种数据结构实现的，每当进入一个函数调用，栈就会加一层栈帧，每当函数返回，栈就会减一层栈帧。由于栈的大小不是无限的，所以，递归调用的次数过多，会导致栈溢出）

示例:

两人年龄相差2  求第m个人  第一个人年龄18



```python
# 两人年龄相差2  求第m个人  第一个人年龄18
def getAge(num):
    if num == 1:
        return 18   # 递归出口
    age = getAge(num - 1) + 2
    return age
print(getAge(6))
```

## 16_匿名函数

> 在Python中，不通过def来声明函数名字，而是通过lambda关键字来定义的函数称为匿名函数。

语法：**lambda 参数：表达式**

先写lambda关键字，然后依次写匿名函数的参数，多个参数中间用逗号连接，然后是一个冒号，冒号后面写返回的表达式。

使用lambda函数可以省去函数的定义，不需要声明一个函数然后使用，而可以在写函数的同时直接使用函数。

### 16.1 示例1(语法方面)

```python
# 匿名函数示例
# 普通
def sum_func(a, b, c):
    return a + b + c
# 匿名
sum_lambda = lambda a, b, c: a + b + c

print(sum_func(1, 100, 10000))
print(sum_lambda(1, 100, 10000))


# 匿名函数调用
# 方法1 同上 使用一个变量接收
a = lambda: print('a')
a()

# 方法二
# 正常调用函数 函数名() 匿名函数的函数名就是函数本身  所以就是(lambad...)()
(lambda: print('hhhh'))()

```

### 16.2 示例2(使用情景)

```python
# 列表 字典排序进阶一丢丢
# sort排序是在原列表内排序的 即影响原列表
stu = [{'name': 'aaa', 'age': '18'},
       {'name': 'bbb', 'age': '22'},
       {'name': 'ccc', 'age': '1'},
       {'name': 'ddd', 'age': '183'}]

stu.sort(key=lambda x: x['name'])  # 按照name排序
print(stu)
print('-----------')
stu.sort(key=lambda x: x['age'])  # 按照age排序
print(stu)

# 没啥用就是看看 
my_list = ['daw', 'dawd', 'fsef', 'grdg']
my_list.sort()
print(my_list)
```

## 17_列表/字典推导式

>Python里面有个很棒的语法糖（syntactic sugar），它就是 `list comprehension` ，有人把它翻译成“列表推导式”，也有人翻译成“列表解析式”。名字听上去很难理解，但是看它的语法就很清晰了。虽然名字叫做 list comprehension，但是这个语法同样适用于dict、set等这一系列可迭代（iterable）数据结构。

目的: 快速创建列表 

语法: 变量 = [生成数据的规则 for 临时变量 in 可迭代对象]

简单解释: 每循环一次就会创建一个数据

### 17.1 列表推导式

1. 列表把一个矩阵（以列表为元素的列表）展平为一个列表
2. 列表嵌套
3. 列表加运算

```python
# 列表推导式

a = [i for i in range(5)]
print(a)

a = [i+i for i in range(5)]
print(a)


b = [f"num:{i}" for i in a]
print(b)

# 把一个矩阵（以列表为元素的列表）展平为一个列表。首先，我们用for循环来实现一下：
list1 = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
]

list2 = []

for i in list1:
    for j in i:
        list2.append(j)
print(list2)
print('-'*10)
# 列表推导式
list2 = [j for i in list1 for j in i]
print(list2)

# 列表推导式
list2 = [i for i in range(10) if i % 2 == 0]
print(list2)

```

### 17.2 字典推导式

语法: 变量 = [key生成数据的规则: value生成数据的规则 for 临时变量 in 可迭代对象]

```python
# 字典推导式
mcase = {'a': 99, 'b': 88, 'A': 9}
mcase_my = {k.lower(): mcase.get(k.lower(), 0) for k in mcase.keys() if
            k.lower() in ['a', 'b']}
print(mcase_my)  # {'a': 99, 'b': 88}
```

# 面向对象

三大特点:

1. 封装性
2. 继承性
3. 多态性

## 01_类语法和简单使用

新式类: 直接或者间接继承object的类。在python3后默认所有类都继承object。 方式一是新式类 二和三是旧式类

旧式类(经典类): 已经过时 不再使用

```python
# 类语法和简单使用
# 类语法规则 class 类名(object):
# 定义方式一
class Dog(object):
    pass
# 定义方式二
class Dog2():
    pass
# 定义方式三
class Dog3:
    pass


"""
新式类: 直接或者间接继承object的类。在python3后默认所有类都继承object。 方式一是新式类 二和三是旧式类

旧式类(经典类): 已经过时 不再使用
"""
```

### 1.1 静态方法

```python
# 静态方法
# 即不需要使用实例属性和类属性的方法
class A(object):
    @staticmethod   
    def testA():
        print("Enter A")


class B(A):  # B继承A
    def testB(self):
        super(B, self).testA()
        print('Leave B')



d = B()
d.testB()
```

## 02_对象

代码中对象是由类创建的。

```python
# 类语法和简单使用
# 类语法规则 class 类名(object):
# 定义方式一
class Dog(object):
    def p(self):
        print('小狗在拆家')


# 创建对象
a = Dog()
# 调用对象的类的方法
a.p()

# 创建列表元组字典类
a = list()
b = tuple()
c = dict()
```

## 03_封装(类进阶一丢丢)

**封装的意义:**

1. 将属性和方法放到一个整体,通过实例化去处理
2. 隐藏内部细节,只需要和对象及其方法交互就可以了
3. 对类的属性和方法增加访问权限控制

**私有权限:**在属性和方法名前加两个下划线

1. 私有属性不能通过对象去访问,但是可以在类内部访问
2. 私有属性和方法不能被子类继承,子类也无法访问
3. 私有属性和方法往往用来处理类内部的事情,起到安全的作用

```python
# 私有方法
# 程序报错 因为testA是私有方法
class A(object):
    def __testA(self):
        print("Enter A")

class B(A):  # B继承A
    def testB(self):
        super(B, self).testA()
        print('Leave B')

d = B()
d.testB()
```



### 3.1 类外部添加和获取对象属性

```python
# 类外部添加和获取对象属性
# 每个对象会报保存自己的属性值  不同对象的属性值不一样
class Dog(object):
    def p(self):
        print('小狗在拆家')
a = Dog()
a.name = '大黄'
a.age = 13
print(a.name)
print(a.age)
b = Dog()
b.name = '二黄'
b.age = '1'
print(b.name)
print(b.age)
```

### 3.2 类内部操作属性

1. self是什么

   ```python
   # self是代表了类自身
   class Dog(object):
       def p(self):
           print(f'self:{self}')
           print(f'self id:{id(self)}')
   
   
   print('-'*10)
   a = Dog()
   a.p()  # self:<__main__.Dog object at 0x000001E27F8DFFD0>
          # self id:2072314249168
          
   print('-'*10)
   print(f'dog:{a}')  # dog:<__main__.Dog object at 0x000001E27F8DFFD0>
   print(f'dog id:{id(a)}')  # dog id:2072314249168
   
   
   # self是代表了类自身
   class Dog(object):
       name = '大黄'
       age = 13
   
       def p(self):
           print(f'name:{self.name}  age:{self.age}')
   
   
   a = Dog()
   a.p()
   
   ```
   
### 3.3 魔法方法
   
> 在python类中,有一类方法以两个下划线开始 两个下划线结束的方法就是魔法方法
   >
   > 并且在满足某些条件的情况下会自动调用
   
1. 魔法方法在什么情况下会自动调用
   2. 魔法方法有什么用处
   3. 魔法方法有什么需要注意的
   

   
`__init__` 在c++或者java中被称为构造函数
   
1. 调用时机: 创建对象后(实例化)自动调用
   2. 作用:
      1.  用来给对象添加属性,给对象一个初始值(构造函数)
      2. 代码业务中,每创建一个对象都需要执行的代码放在init中
   
```python
   # __init__
   class Dog(object):
   
       def __init__(self):
           print("我被初始化了")
   
   
   a = Dog()  # 我被初始化了
   print('-' * 10)
   
   
   class Dog1(object):
       def __init__(self, name):
           print(f'name: {name}')
   
   
   b = Dog1('大黄')  # name: 大黄
   ```
   
`__str__`
   
>  没有定义__str__方法时候返回的是对象的引用地址,有点类似于java的toString()方法
   
1. 调用时机: 
      1. print(对象)时候会自动调用`__str__`方法,打印输出的结果是`__str__`的返回值 
      2. `str(对象)`类型转化,将自定义对象转化为字符串时候,会自动调用
   2. 作用:
      1.  打印对象时候可以输出一些属性
      2. 需要将对象转化为字符串类型的时候
   3. 注意点: 方法返回必须是一个字符串,只有self一个参数
   
```python
   # __init__方法演示 类似于java的toString方法
   class Dog1(object):
       def __init__(self, name, age):
           self.name = name  # 添加属性 相当于java的带参构造
           self.age = age  # 添加属性 相当于java的带参构造
   
       def __str__(self):
           return f'name: {self.name} age:{self.age}'
   
   
   b = Dog1('大黄', 12)  # name: 大黄
   print(b)  # name: 大黄 age:12
   
   strB = str(b)
   print(strB)  # name: 大黄 age:12
   ```
   
`__del__`
   
类似于c++的析构函数
   
1. 调用时机:  对象在内存中被销毁时候(引用计数为0)会自动调用
   1. 程序代码结束的时候
   2. 使用`__del__`将这个对象的引用计数设置为0会自动调用
2. 作用:  内存管理
3. 注意点: 方法返回必须是一个字符串,只有self一个参数

**值得一提的是:基本数据类型中内置了`__str__`,但是自定义类型的列表,外层的列表有__str__内层的自定义类型则还是引用地址,这时候需要在自定义类中添加`__repr__`方法 **

## 04_继承

​		面向对象编程 (OOP) 语言的一个主要功能就是“继承”。继承是指这样一种能力：它可以使用现有类的所有功能，并在无需重新编写原来的类的情况下对这些功能进行扩展。

　　通过继承创建的新类称为“子类”或“派生类”，被继承的类称为“基类”、“父类”或“超类”，继承的过程，就是从一般到特殊的过程。在某些 OOP 语言中，一个子类可以继承多个基类。但是一般情况下，一个子类只能有一个基类，要实现多重继承，可以通过多级继承来实现。

　　继承概念的实现方式主要有2类：实现继承、接口继承。

1. 实现继承是指使用基类的属性和方法而无需额外编码的能力。
2.  接口继承是指仅使用属性和方法的名称、但是子类必须提供实现的能力(子类重构爹类方法)。

　　在考虑使用继承时，有一点需要注意，那就是两个类之间的关系应该是“属于”关系。例如，Employee 是一个人，Manager 也是一个人，因此这两个类都可以继承 Person 类。但是 Leg 类却不能继承 Person 类，因为腿并不是一个人。

### 4.1 继承的基本语法

class 子类名(父类名):

```python
# 继承语法
class Person(object):  # 定义一个父类

    def talk(self):  # 父类中的方法
        print("person is talking....")


class Chinese(Person):  # 定义一个子类， 继承Person类

    def walk(self):  # 在子类中定义其自身的方法
        print('is walking...')


c = Chinese()
c.talk()  # 调用继承的Person类的方法 person is talking....
c.walk()  # 调用本身的方法  person is talking....
```

**##** **4.2 构造函数的继承**

如果我们只是简单的在子类Chinese中定义一个构造函数，其实就是在重构。

这样子类就不能继承父类的属性了。所以我们在定义子类的构造函数时，要先继承再构造，这样我们也能获取父类的属性了。

过程: 在子类的构造函数内第一句`super(Chinese,self).__init__(name,age)`继承父类的构造,然后再添加自己的方法

```python
# 构造函数继承
# 如果我们只是简单的在子类Chinese中定义一个构造函数，其实就是在重构。
# 这样子类就不能继承父类的属性了。所以我们在定义子类的构造函数时，要先继承再构造，这样我们也能获取父类的属性了。 
class Person(object):

    def __init__(self, name, age):
        self.name = name
        self.age = age
        self.weight = 'weight'

    def talk(self):
        print("person is talking....")

class Chinese(Person):
    def __init__(self, name, age, language):  # 先继承，在重构
        Person.__init__(self, name, age)  # 继承父类的构造方法，也可以写成：super(Chinese,self).__init__(name,age)
        self.language = language  # 定义类的本身属性
    def walk(self):
        print('is walking...')

c = Chinese('bigberg', 22, 'Chinese')
```



### 4.3 单继承和多继承

多继承使用`__mro__`可以查看运行顺序

```python
# 单继承
class A(object):
    def __init__(self):
        print('i am A')

class B(A):
    def __init__(self):
        super(B, self).__init__()  # 在__init__()方法中确保父类被正确的初始化了
        print('i am B')

b = B()  #  i am A   \n  i am B


# 多继承
class A(object):
    def __init__(self):
        print("Enter A")

class B(A):    # B继承A
    def __init__(self):
        print('Enter B')
        super(B, self).__init__()
        print('Leave B')

class C(A):  # C继承A
    def __init__(self):
        print('Enter C')
        super(C, self).__init__()
        print('Leave C')

class D(B, C):  # D继承B,C
    def __init__(self):
        print('Enter D')
        super(D, self).__init__()
        print("Leave D")

d = D()
print(D.__mro__)  # (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)
print(C.__mro__)  # (<class '__main__.C'>, <class '__main__.A'>, <class 'object'>)
print(B.__mro__)  # (<class '__main__.B'>, <class '__main__.A'>, <class 'object'>)
print(A.__mro__)  # (<class '__main__.A'>, <class 'object'>)

"""
结果:
Enter D
Enter B
Enter C
Enter A
Leave C
Leave B
Leave D

"""

# 示例二
class Dog(object):
    def bark(self):
        print('汪汪汪叫')

    def eat(self):
        print('啃骨头')


class God(object):
    def play(self):
        print('在空中漂一会')

    def eat(self):
        print('吃蟠桃仙丹')


# class XTQ(Dog, God):
#     pass
class XTQ(Dog, God):
    def eat(self):
        super(Dog, self).eat()
        # super(God, self).eat()  报错  严格按照执行链的父类 God的父类是object object中没有eat方法就报错
    pass


a = XTQ()
a.eat()

print(XTQ.__mro__)  # (<class '__main__.XTQ'>, <class '__main__.Dog'>, <class '__main__.God'>, <class 'object'>)


```



### 4.4 子类重写父类的同名方法

```python
# 子类重写父类同名方法

class father(object):
    def name(self):
        print('i am father')

class son(father):
    def name(self):
        print('i am son')


son().name()  # i am son
```



### 4.5 子类调用父类的同名方法

```python
# 子类调用父类同名方法

class father(object):
    def name(self):
        print('i am father')


class son(father):
    def name(self):
        print('i am son')

    def call(self):
        father.name(self)   # 方法一
        super(son, self).name()  # 方法二
        super().name()  # 方法三


son().call()  # i am father  \n   i am son
```

### 4.6 继承中的init

父类中构造函数含参 子类不含  调用时候会优先子类构造 父类的属性则不能使用

如果子类重写了init方法需要调用父类的init方法,给对象添加父类继承的属性

即:  先继承再构造  

```python
class father(object):
    def __init__(self, sex):
        print('father man')


class son(father):
    def __init__(self, name):
        print('son hhh', f'son {name}')


son('hahahah')  # i am f
```

## 05_多态

**让具有不同功能的函数可以使用相同的函数名，这样就可以用一个函数名调用不同内容(功能)的函数。**

**Python中多态的特点**

1. 只关心对象的实例方法是否同名，不关心对象所属的类型；
2. 对象所属的类之间，继承关系可有可无；
3. 多态的好处可以增加代码的外部调用灵活度，让代码更加通用，兼容性比较强；
4. 多态是调用方法的技巧，不会影响到类的内部设计。

```python
# 多态

class Dog(object):
    def __init__(self, name):
        self.name = name

    def play(self):
        print(f'小狗{self.name} 在玩耍....')


class XTQ(Dog):
    def play(self):
        print(f'小狗{self.name} 在天生追云....')


def play_with_dog(obj_dog):
    obj_dog.play()


dog = Dog('大黄')
print(play_with_dog(dog))

dog = XTQ('大黄')
print(play_with_dog(dog))
```

# 异常

## 01_什么是异常

异常即是一个事件，该事件会在程序执行过程中发生，影响了程序的正常执行。

一般情况下，在Python无法正常处理程序时就会发生一个异常。

异常是Python对象，表示一个错误。

当Python脚本发生异常时我们需要捕获处理它，否则程序会终止执行。

## 02_异常捕获

### 2.1 语法

```python
try:
    可能发生异常的代码
except (异常类1,异常类2):
    发生异常执行的代码
```

## 2.2 示例

```python
# 异常

# 多异常写法1  多异常同时处理
a = input("please input NUM:")
try:
    b = 10 / int(a)
    print('计算的结果是:', b)
except (ZeroDivisionError, ValueError):
    print('不能输入0')

# 多异常写法2  多异常分开处理
a = input("please input NUM:")
try:
    b = 10 / int(a)
    print('计算的结果是:', b)
except ZeroDivisionError:
    print('不能输入0')
except ValueError:
    print('请输入数字')
```

### 2.3 打印异常错误信息

```python
a = input("please input NUM:")
try:
    b = 10 / int(a)
    print('计算的结果是:', b)
except (ZeroDivisionError, ValueError) as e:  # 打印错误信息
    print('您的输出有误', e)
```

### 2.4 捕获所有异常

except后不跟异常类

```python
a = input("please input NUM:")
try:
    b = 10 / int(a)
    print('计算的结果是:', b)
except:  # 打印错误信息
    print('您的输出有误', )
```

### 2.5 异常完整结构

```python
try:
    可能发生异常的代码
except (异常类1,异常类2):
    发生异常执行的代码
else:
	没有发生异常执行的代码
finally:
    不管有没有发生异常都会执行
```

# 模块

就是import后的东西  java的包







