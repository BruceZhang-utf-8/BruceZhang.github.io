---
title: python模块和包管理
date: 2023-10-21 14:59:56
tags:
 - python
categories:
 - python
 - python基础
---

# 模块和包管理

## 一 模块的导入

导入内置模块

```python
import math
```

math是Python标准库中的一个模块，用来进行数学运算，导入后可以使用它的功能

```python
# 求一个数的平方根
math.sqrt(4)
#使用dir函数查看math模块所有的函数
print(dir(math))
```

使用from ... import ... 语句具体导入某一个子模块或函数

```python
from math import sqrt

sqrt(4)
```

注意变量的重名问题，如果代码中本来就有一个sqrt的函数，可以使用as 关键字给模块起一个别名

```python
from math import sqrt as squart

def sqrt(num):
	pass
squart(4)
```

或者这样

```python
import math as math2

math = 0
math2.sqrt(4)
```

一次性导入多个模块

```python
import sys,os
from math import sqrt,pow
```

**注意：**

1. 在实际调用模块之前，必须先导入，否则会报错
2. 导入一个不存在的模块，也会报错

## 二 自定义模块

除了使用标准库外，还可以自定义模块

以一个案例为例：实际的文件结构如下：

```python
└── project
   ├── cart.py # Cart类
   ├── goods.py # Goods类
   └── main.py # 入口文件
```

上面三个类中，不同的类在不同文件内，main.py

```python
from carts import Cart
from goods import Good

g1 = Good("ipone14pro", 5000, 0.95)
g2 = Good("华为mate60", 9000, 0.9)
g3 = Good("oppo", 4999, 0.8)

cart = Cart()
cart.add(g1)
cart.add(g2, 2)
cart.add(g3, 5)
print(cart.cart)
print("商品的总价值是%.2f" % cart.total_amount)
print("商品的实际支付的价格是%.2f" % cart.pay_amount)
print("%s的折扣后价格是%.2f" % (g1.name, g1.calc_price()))
cart.show()
```

main.py作为项目的入口文件，运行时从它这里启动

```python
python main.py
```

python解释器时怎么找到cart和good模块?因为它们和main.py在同一个目录下，所以可以自动的发现它们。那如果有很多这样的模块，我们想把它们放到同一个目录下，以便统一管理，该怎么做呢? 比如，需要将cart.py 和 goods.py放到shopcart包内，调整目录结构如下：

```python
├── project
│ ├── main.py
│ └── shopcart
│   ├── __init__.py
│ 	├── cart.py
│ 	└── goods.py
```

在mian.py同级目录下，多了一个shopcart目录，注意，shopcart目录里除了cart.py和goods.py目录，还多了一个 "__init__.py" ，这是一个空文件，它的文件名看起来很奇怪，这是Python规定的命名规范，只要目录有一个名为 __init__.py 的文件，则将这个目录视为包(package)，现在修改main.py，作一下调整

```python
from shopcart.cart import Cart
from shopcart.goods import Goods
```

同时将下面的部分也做一下调整

```python
if __name__ == '__main__':
    g1 = Goods('iPhone 11', 6000, 0.9)
    g2 = Goods('U盘32G', 100, 0.8)
    g3 = Goods('华为P40', 5000)
    cart = Cart()
    cart.add(g1)
    cart.add(g2, 3)
    cart.show()
```

__name__表示当前文件所在模块的名称，模块可以通过检查自己的__name__来得知是否在main的作用域中，这使得模块可以在作为脚本运行时条件性的执行一些代码，而在被import时不会】执行

## 三 常用内置模块

### 3.1 日期实践类型

datetime 模块中包含了几个常用的日期时间类，最常用的是datetime和timedelta。

> 注意：我们下面使用的datetime是指datetime类而不是datetime模块

```python
from datetime import datetime,timedelta
#返回当前时间的datetime对象
now = datetime.now()
type(now)
#查看当前时间的年月日
print(now.year,now.month,now.day)
#查看当前时间的时间戳，精确到微秒
now.timestamp()
```

datetime也提供将日期时间对象和字符串相互转换的操作

```python
# 返回指定格式的日期字符串
datetime.now().strftime('%Y-%m-%d %H:%M:%S')
# 将指定格式的字符串转换为日期时间对象
datetime.strtftime('2020-01-01 00:00:00','%Y-%m-%d %H:%M:%S')

```

下面是一些常用的代码含义：

| 代码 |                    含义                     |      示例       |
| :--: | :-----------------------------------------: | :-------------: |
|  %Y  |         十进制数表示的带世纪的年份          |   2023，2022    |
|  %m  |         补零后，以十进制显示的月份          |   01，02...12   |
|  %d  |   补零后，以十进制数显示的月份中的一天。    | 01, 02, ..., 31 |
|  %H  | 以补零后的十进制数表示的小时（24 小时制）。 | 00, 01, ..., 23 |
|  %M  |       补零后，以十进制数显示的分钟。        | 00, 01, ..., 59 |
|  %S  |        补零后，以十进制数显示的秒。         | 00, 01, ..., 59 |

只要得到了datetime对象，就可以把它转换成各种格式。同样，只要有一个相对标准化的格式，就可以将它转换为datetime对象。

```python
now = datetime.now()
last_year = now.replace(year=2019)
print(last_year.strftime('%Y-%m-%d %H:%M:%S'))
```

两个datetime对象之间想差多长时间，可以将这两个对象相减

```python
delta = now - last_year
type(delta)
```

得到的对象是一个timedelta对象，我们可以根据timedelta对象知道这两个使劲啊相差多少天多少分多少秒

```
delta.days
delta.seconds
```

现在得到delta就是一个timedelta对象， 它表示366天整的时间，我们也可以将一个datetime对象和一个timedelta对象相加，将会得到一个新的datetime对象：

```python
now + delta
```

将当前时间加上366天，就是明年的明天。timedelta类提供了非常便捷的方式帮我们处理日期时间，比如我们想要构造：

```python
# 从当前开始20天后的时间
datetime.now() + timedelta(days=20)
# 两个半小时之前的时间
datetime.now() - timedelta(hours=2, minutes=30)
```

**time-时间的访问和转换**

另外一个常用的时间模块

```python
import time
# 返回当前时间戳
time.time()
# 返回当前时间的格式化字符串
time.strftime('%Y-%m-%d %H:%M:%S')
```

让程序sleep一会

```python
time.sleep(3)
```

sleep函数会将当前的程序暂停若干秒数。

### 3.2 random-随机数

默认random模块会根据当前的系统时间作为随机数种子，所以可以保证生成的随机数不会重复。

```python
# 生成一个随机浮点数，范围[0.0, 1.0)
random.random()
# 生成1到100之间的随机整数，包括1和100
random.randint(1, 100)
# 从序列中随机抽出一个元素
random.choice(['a', 'b', 'c', 'd', 'e', 'f', 'g'])
# 从序列中随机抽出k个元素,注意抽出来的元素可能会重复
random.choices(['a', 'b', 'c', 'd', 'e', 'f', 'g'], k=2)
# 跟choices函数类似，但它是不重复的随机抽样
random.sample(['a', 'b', 'c', 'd', 'e', 'f', 'g'])
# 将一个序列随机打乱，注意这个序列不能是只读的
lst = ['a', 'b', 'c', 'd', 'e', 'f', 'g']
random.shuffle(lst)
```

### 3.3 os-操作系统接口

os模块提供了一种使用与操作系统相关的功能的便捷式途径。

```python
# 获取当前目录的路径
os.getcwd()
# 创建指定目录
os.mkdir(path)
# 与 mkdir() 类似，但会自动创建到达最后一级目录所需要的中间目录,即可以递归创建。
os.makedirs(path)
# 返回一个列表，该列表包含了 path 中所有文件与目录的名称。
os.listdir()
```

另一个很常用的子模块就是os.path，它提供了常用的路径操作。

```python
# 显示当前目录的绝对路径
os.path.abspath('./')
os.path.abspath("__file__")
```

> 在大部分操作系统中，一般用 . 表示当前目录，用 .. 表示父级目录
>
> 相对路径：相对于当前目录的路径
>
> 绝对路径：以根目录为开始的路径（windows和Mac、Linux的根目录不同）
>
> 目录分隔符：windows 是 \ , Mac 和 Linux中是 /

```python
# 如果 path 是 现有的 目录，则返回 True。
os.path.isdir(path)
# 如果 path 是 现有的 常规文件，则返回 True。
os.path.isfile()
# 目录分隔符
os.sep
# 合理地拼接一个或多个路径部分。
os.path.join(path, *paths)
# 返回路径 path 的目录名称
os.path.dirname("/tmp/test.txt") # '/tmp'
# 返回路径 path 的基本名称，文件名或是最后一级的目录名
os.path.basename("/tmp/test.txt") # 'test.txt'
os.path.basename("/tmp/test") # 'test'
```

### 3.4 系统相关参数和函数

sys.path ，它返回的是一个列表，包含了若干个路径，它表示的是Python查找包的路径顺序，第一个是一个空字符串，它表示当前目录。之所以我们使用import 语句可以导入模块，靠的就是它。

```python
sys.path
['',
'/Users/envs/py3/bin',
'/Users/envs/py3/lib/python36.zip',
'/Users/envs/py3/lib/python3.6',
'/Users/envs/py3/lib/python3.6/lib-dynload',
'/Users/envs/py3/lib/python3.6/site-packages',
'/Users/envs/py3/lib/python3.6/site-packages/pyspider-0.3.10-py3.6.egg',
'/Users/envs/py3/lib/python3.6/site-packages/setuptools-27.2.0-py3.6.egg',
'/Users/envs/py3/lib/python3.6/site-packages/IPython/extensions',
'/Users/.ipython']
```

sys.argv 表示启动的时候传递给Python脚本的命令行参数。

```python
import sys
if __name__ == '__main__':
	print("Hello", end=' ')
	if len(sys.argv) > 1:
		print(' '.join(sys.argv[1:]))
```

上述是一个Python脚本 hello.py 的代码，现在我们试着用不同的方式启动它

```python
python hello.py

python hello.py Bill

python hello.py Mr Gates
```

```python
# sys.argv是个列表
type(sys.argv)
print(sys.argv)
```

可以看到sys.argv是一个列表，它的第一个元素就是脚本的文件名。所以传递它的启动参数，都会放在列表的后面。我们可以使用这种方式接收用户传递的参数。

