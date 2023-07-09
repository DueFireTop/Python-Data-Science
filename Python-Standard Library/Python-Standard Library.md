# 1. Python标准库

![image-20230323201544110](https://bucket-zly.oss-cn-beijing.aliyuncs.com/img/Typora/202303232015192.png)

## 1.1 time

<img src="https://bucket-zly.oss-cn-beijing.aliyuncs.com/img/Typora/202303241943382.png" alt="image-20230324194310267" style="zoom:67%;" />

Python处理时间的标准库

### 1.1.1 获取现在的时间

1. time.localtime()——本地时间
2. time.gmtime()——UTC世界统一时间

北京时间比世界统一时间UTC早8个小时

```python
import time

print(time.localtime())
print(time.gmtime())
'''
time.struct_time(tm_year=2023, tm_mon=3, tm_mday=23, tm_hour=20, tm_min=23, tm_sec=5, tm_wday=3, tm_yday=82, tm_isdst=0)
time.struct_time(tm_year=2023, tm_mon=3, tm_mday=23, tm_hour=12, tm_min=23, tm_sec=5, tm_wday=3, tm_yday=82, tm_isdst=0)
'''
```

```python
print(time.ctime()) # 返回本地时间的字符串
'''
Thu Mar 23 20:29:00 2023
'''
```

### 1.1.2 时间戳与计时器

1. time.time()——返回自纪元以来的秒数，记录sleep

2. time.perf_counter()——随意选取一个时间点，记录现在时间到该时间点的间隔秒数，记录sleep

   perf_counter() 精度较 time() 更高一些

3. time.process_time()——随意选取一个时间点，记录现在时间到该时间点的间隔秒数，不记录sleep

```python
t_1_start = time.time()
t_2_start = time.perf_counter()
t_3_start = time.process_time()

res = 0

for i in range(1000000):
    res += i

time.sleep(5)

t_1_end = time.time()
t_2_end = time.perf_counter()
t_3_end = time.process_time()

print("time: {:.3f}秒".format(t_1_end - t_1_start))
print("perf_counter: {:.3f}秒".format(t_2_end - t_2_start))
print("process_time: {:.3f}秒".format(t_3_end - t_3_start))

'''
time: 5.090秒
perf_counter: 5.089秒
process_time: 0.078秒
'''
```

### 1.1.3 格式化

- time.strftime——自定义格式化输出

```python
loc_time = time.localtime()

print(time.strftime("%Y-%m-%d %A %H:%M:%S", loc_time))
'''
2023-03-23 Thursday 20:41:16
'''
```

### 1.1.4 睡觉

- time.sleep()



---

## 1.2 random

![image-20230324194324980](https://bucket-zly.oss-cn-beijing.aliyuncs.com/img/Typora/202303241943089.png)

随机数再计算机应用中十分常见

Python通过random库提供各种伪随机数

基本可以用于除加密解密算法歪的大多数工程应用

### 1.2.1 随机种子——seed(a = None)

1. 相同种子会产生相同的随机数
2. 如果不设置随机种子，以系统当前时间为默认值

```python
seed(10)
print(random())
seed(10)
print(random())
print(random())
'''
0.5714025946899135
0.5714025946899135
0.4288890546751146
'''
```

### 1.2.2 产生随机整数

1. randint(a, b)——产生 [a, b] 之间的随机整数

   ```python
   numbers = [randint(1, 10) for i in range(10)]
   print(numbers)
   '''
   [10, 1, 4, 8, 8, 5, 3, 1, 9, 8]
   '''
   ```

2. randrange(a)_产生[0, a)之间的随机整数

   ```python
   numbers = [randrange(10) for i in range(10)]
   print(numbers)
   '''
   [5, 1, 3, 5, 0, 6, 2, 9, 5, 6]
   '''
   ```

3. randrange(a, b, step)——产生[a, b)之间以step为步长的随机整数

   ```python
   numbers = [randrange(0, 10, 2) for i in range(10)]
   print(numbers)
   '''
   [6, 4, 4, 6, 2, 4, 4, 2, 6, 2]
   '''
   ```

### 1.2.3 产生随机浮点数

1. random()——产生[0.0, 1.0)之间的随机浮点数

   ```python
   numbers = [random() for i in range(10)]
   print(numbers)
   '''
   [0.9693881604049188, 0.613326820546709, 0.0442606328646209, 0.004055144158407464, 0.13397252704913387, 0.941002271395834, 0.3028605620290723, 0.3661456016604264, 0.8981962445391883, 0.31436380495645067]
   
   '''
   ```

2. uniform(a, b)——产生[a, b]之间的随机浮点数

   ```python
   numbers = [uniform(2.1, 3.5) for i in range(10)]
   print(numbers)
   '''
   [2.8685750576173676, 2.710443340673771, 2.190991846577591, 2.9183647159827024, 3.281695056726663, 2.318986485742369, 2.414018556160458, 2.678018290800777, 2.1516948166820806, 2.7952448980631672]
   '''
   ```

### 1.2.4 序列用函数

1. choice(seq)——从序列类型中随机返回一个元素

   ```python
   print(choice(['win', 'lose', 'draw']))
   print(choice("python"))
   '''
   draw
   n
   '''
   ```

2. choices(seq, weights = None, k)——对序列类型进行k次重复采样，可设置权重

   权重的意思是随机取到的概率，权重越大概率越高

   ```python
   print(choices(['win', 'lose', 'draw'], k=5))
   print(choices(['win', 'lose', 'draw'], [4, 4, 2] , k=10))
   '''
   ['win', 'draw', 'win', 'lose', 'draw']
   ['draw', 'win', 'draw', 'lose', 'lose', 'draw', 'win', 'win', 'draw', 'lose']
   '''
   ```

3. shuffle(seq)——将序列类型中元素随机排列，返回打乱后的序列

   ```python
   numbers = ["one", "two", "three", "four"]
   shuffle(numbers)
   print(numbers)
   '''
   ['two', 'four', 'one', 'three']
   '''
   ```

4. sample(pop, k)——从pop类型中随机选取k个元素，以列表类型返回

   ```python
   print(sample([10, 20, 30, 40, 50], k=3))
   '''
   [10, 30, 20]
   '''
   ```

5. 概率分布——以高斯分布为例

   - gauss(mean, std)——生产一个符合高斯分布的随机数

     ```python
     number = gauss(0, 1)
     print(number)
     '''
     -0.4766824049599741
     '''
     ```

     ```python
     import matplotlib.pyplot as plt
     
     res = [gauss(0, 1) for i in range(100000)]
     plt.hist(res, bins=1000)
     plt.show()
     ```

     ![image-20230323210829011](https://bucket-zly.oss-cn-beijing.aliyuncs.com/img/Typora/202303232108066.png)

   - 【例】发红包

     ```python
     import random
     
     def red_packet(total, num):
         for i in range(1, num):
             per = random.uniform(0.01, total / (num - i + 1) * 2)
             total = total - per
             print("第{}红包金额：{:.2f}元".format(i, per))
         else:
             print("第{}位红金额：{:.2f}元".format(num, total))
     
     red_packet(10, 5)
     '''
     第1红包金额：3.77元
     第2红包金额：0.55元
     第3红包金额：0.67元
     第4红包金额：3.87元
     第5位红金额：1.13元
     '''
     ```

   - 【例】生产4位由数字和英文字母构成的验证码

     ```python
     import random
     import string
     
     print(string.digits)
     print(string.digits + string.ascii_letters)
     
     s = string.digits + string.ascii_letters
     v = random.sample(s, 4)
     
     print(v)
     print(''.join(v))
     '''
     0123456789
     0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ
     ['k', 'r', 'e', '0']
     kre0
     '''
     ```



---

## 1.3 collections——容器数据类型

![image-20230324194408586](https://bucket-zly.oss-cn-beijing.aliyuncs.com/img/Typora/202303241944675.png)

```python
import collections
```

### 1.3.1 namedtuple——具名元组

- 点的坐标，仅看数据，很难知道表达的是一个点的坐标

  ```python
  p = (1, 2)
  ```

- 构建一个新的元组子类

  定义方法如下：typename 是元组名字，field_names 是域名

  ```python
  collections.namedtuple(typename, field_names, *, rename=False, defaults=None, module=None)
  ```

  ```python
  Point = collections.namedtuple("Point", ["x", "y"])
  p = Point(1, y=2)
  print(p)
  '''
  Point(x=1, y=2)
  '''
  ```

- 可以调用属性

  ```python
  print(p.x)
  print(p.y)
  ```

- 有元组的性质

  ```python
  print(p[0])
  print(p[1])
  x, y = p
  print(x)
  print(y)
  '''
  1
  2
  1
  2
  '''
  ```

- 确实是元组的子类

  ```python
  print(isinstance(p, tuple))
  '''
  True
  '''
  ```

- 【例】模拟扑克牌

  ```python
  Card = collections.namedtuple("Card", ["rank", "suit"])
  ranks = [str(n) for n in range(2, 11)] + list("JQKA") # 牌的大小
  suits = "spades diamonds cluns jearts".split() # 牌的花色
  print("ranks: ", ranks)
  print("suits: ", suits)
  cards = [Card(ranks, suits) for rank in ranks
           for suit in suits]
  print(cards)
  '''
  ranks:  ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A']
  suits:  ['spades', 'diamonds', 'cluns', 'jearts']
  [Card(rank='2', suit='spades'),
  Card(rank='2', suit='diamonds'),
  Card(rank='2', suit='cluns'),
  Card(rank='2', suit='jearts'), 
  Card(rank='3', suit='spades'), 
  Card(rank='3', suit='diamonds'),
  Card(rank='3', suit='cluns'), 
  Card(rank='3', suit='jearts'), 
  Card(rank='4', suit='spades'), 
  Card(rank='4', suit='diamonds'),
  Card(rank='4', suit='cluns'), 
  Card(rank='4', suit='jearts'),
  Card(rank='5', suit='spades'),
  Card(rank='5', suit='diamonds'),
  Card(rank='5', suit='cluns'),
  Card(rank='5', suit='jearts'),
  Card(rank='6', suit='spades'), 
  Card(rank='6', suit='diamonds'),
  Card(rank='6', suit='cluns'), 
  Card(rank='6', suit='jearts'), 
  Card(rank='7', suit='spades'), 
  Card(rank='7', suit='diamonds'), 
  Card(rank='7', suit='cluns'), 
  Card(rank='7', suit='jearts'), 
  Card(rank='8', suit='spades'),
  Card(rank='8', suit='diamonds'),
  Card(rank='8', suit='cluns'), 
  Card(rank='8', suit='jearts'),
  Card(rank='9', suit='spades'),
  Card(rank='9', suit='diamonds'),
  Card(rank='9', suit='cluns'), 
  Card(rank='9', suit='jearts'),
  Card(rank='10', suit='spades'),
  Card(rank='10', suit='diamonds'),
  Card(rank='10', suit='cluns'), 
  Card(rank='10', suit='jearts'),
  Card(rank='J', suit='spades'),
  Card(rank='J', suit='diamonds'),
  Card(rank='J', suit='cluns'),
  Card(rank='J', suit='jearts'),
  Card(rank='Q', suit='spades'),
  Card(rank='Q', suit='diamonds'),
  Card(rank='Q', suit='cluns'),
  Card(rank='Q', suit='jearts'),
  Card(rank='K', suit='spades'),
  Card(rank='K', suit='diamonds'),
  Card(rank='K', suit='cluns'), 
  Card(rank='K', suit='jearts'),
  Card(rank='A', suit='spades'),
  Card(rank='A', suit='diamonds'),
  Card(rank='A', suit='cluns'), 
  Card(rank='A', suit='jearts')]
  '''
  ```

  ```python
  # 洗牌
  from random import *
  shuffle(cards)
  print(cards)
  '''
  [Card(rank='7', suit='jearts'), Card(rank='10', suit='cluns'), Card(rank='9', suit='spades'), Card(rank='8', suit='spades'), Card(rank='K', suit='spades'), Card(rank='5', suit='diamonds'), Card(rank='K', suit='diamonds'), Card(rank='6', suit='spades'), Card(rank='10', suit='jearts'), Card(rank='3', suit='spades'), Card(rank='Q', suit='diamonds'), Card(rank='6', suit='jearts'), Card(rank='2', suit='diamonds'), Card(rank='5', suit='spades'), Card(rank='A', suit='spades'), Card(rank='J', suit='jearts'), Card(rank='A', suit='diamonds'), Card(rank='4', suit='cluns'), Card(rank='8', suit='diamonds'), Card(rank='10', suit='diamonds'), Card(rank='5', suit='jearts'), Card(rank='3', suit='diamonds'), Card(rank='6', suit='diamonds'), Card(rank='A', suit='jearts'), Card(rank='2', suit='spades'), Card(rank='8', suit='cluns'), Card(rank='10', suit='spades'), Card(rank='9', suit='jearts'), Card(rank='7', suit='diamonds'), Card(rank='5', suit='cluns'), Card(rank='J', suit='spades'), Card(rank='Q', suit='jearts'), Card(rank='7', suit='spades'), Card(rank='3', suit='cluns'), Card(rank='7', suit='cluns'), Card(rank='3', suit='jearts'), Card(rank='Q', suit='spades'), Card(rank='4', suit='spades'), Card(rank='4', suit='diamonds'), Card(rank='J', suit='cluns'), Card(rank='9', suit='cluns'), Card(rank='2', suit='cluns'), Card(rank='6', suit='cluns'), Card(rank='Q', suit='cluns'), Card(rank='A', suit='cluns'), Card(rank='2', suit='jearts'), Card(rank='8', suit='jearts'), Card(rank='K', suit='cluns'), Card(rank='J', suit='diamonds'), Card(rank='9', suit='diamonds'), Card(rank='4', suit='jearts'), Card(rank='K', suit='jearts')]
  '''
  ```

  ```python
  # 随机抽一张牌
  print(choice(cards))
  '''
  Card(rank='7', suit='cluns')
  '''
  ```

### 1.3.2 Counter——计数器

```python
from collections import Counter

s = "牛奶奶找刘奶奶买牛奶"
colors = ["red", "blue", "red", "green", "blue", "blue"]
count_str = Counter(s)
count_color = Counter(colors)

print(count_str)
print(count_color)
'''
Counter({'奶': 5, '牛': 2, '找': 1, '刘': 1, '买': 1})
Counter({'blue': 3, 'red': 2, 'green': 1})
'''
```

- 是字典的一个子类

  ```python
  print(isinstance(Counter(), dict))
  '''
  True
  '''
  ```

- 最常见的统计——most_common(n)

  提供n个频率最高的元素和计数

  ```python
  print(count_color.most_common(1))
  '''
  3
  '''
  ```

- 元素展开——elements()

  ```python
  print(list(count_str.elements()))
  '''
  ['牛', '牛', '奶', '奶', '奶', '奶', '奶', '找', '刘', '买']
  '''
  ```

- 其他的一些加减操作

  ```python
  c = Counter(a = 3, b = 1)
  d = Counter(a = 1, b = 2)
  print(c + d)
  '''
  Counter({'a': 4, 'b': 3})
  '''
  ```

- 【例】从一副牌中抽取10张，大于10的比例有多少

  ```python
  cards = collections.Counter(tens = 16, low_cards = 36)
  seen = sample(list(cards.elements()), k = 20)
  print(seen)
  print(seen.count('tens') / 20)
  '''
  ['low_cards', 'low_cards', 'tens', 'tens', 'low_cards', 'low_cards', 'low_cards', 'low_cards', 'low_cards', 'tens', 'tens', 'low_cards', 'low_cards', 'low_cards', 'low_cards', 'low_cards', 'tens', 'tens', 'low_cards', 'low_cards']
  0.3
  '''
  ```

### 1.3.3 deque——双向队列

列表访问数据非常快速

插入和删除操作非常慢——通过移动元素位置来实现

特别是insert(0, v) 和 pop(0)，在列表开始进行插入和删除操作

- 双向队列可以方便的在队列两边高效、快速的增加和删除元素

  ```python
  from collections import deque
  
  d = deque('cde')
  print(d)
  '''
  deque(['c', 'd', 'e'])
  '''
  ```

  ```python
  d.append("f") # 右端增加
  d.append("s")
  d.appendleft("b") # 左端增加
  d.appendleft("a")
  print(d)
  '''
  deque(['a', 'b', 'c', 'd', 'e', 'f', 's'])
  
  '''
  ```

  ```python
  d.pop() # 右端删除
  d.popleft() # 左端删除
  print(d)
  '''
  deque(['b', 'c', 'd', 'e', 'f'])
  '''
  ```



---

## 1.4 itertools——迭代器

![image-20230324194421682](https://bucket-zly.oss-cn-beijing.aliyuncs.com/img/Typora/202303241944780.png)

### 1.4.1 排列组合迭代器【看】

1. product——笛卡尔积

   ```python
   import itertools
   
   for i in itertools.product("ABC", "01"):
       print(i)
   '''
   ('A', '0')
   ('A', '1')
   ('B', '0')
   ('B', '1')
   ('C', '0')
   ('C', '1')
   '''
   ```

   ```python
   for i in itertools.product("ABC", repeat=3):
       print(i)
   '''
   ('A', 'A', 'A')
   ('A', 'A', 'B')
   ('A', 'A', 'C')
   ('A', 'B', 'A')
   ('A', 'B', 'B')
   ('A', 'B', 'C')
   ('A', 'C', 'A')
   ('A', 'C', 'B')
   ('A', 'C', 'C')
   ('B', 'A', 'A')
   ('B', 'A', 'B')
   ('B', 'A', 'C')
   ('B', 'B', 'A')
   ('B', 'B', 'B')
   ('B', 'B', 'C')
   ('B', 'C', 'A')
   ('B', 'C', 'B')
   ('B', 'C', 'C')
   ('C', 'A', 'A')
   ('C', 'A', 'B')
   ('C', 'A', 'C')
   ('C', 'B', 'A')
   ('C', 'B', 'B')
   ('C', 'B', 'C')
   ('C', 'C', 'A')
   ('C', 'C', 'B')
   ('C', 'C', 'C')
   '''
   ```

2. permutations——排列

   ```python
   # 3是排列的长度
   for i in itertools.permutations("ABCD", 3):
       print(i)
   '''
   ('A', 'B', 'C')
   ('A', 'B', 'D')
   ('A', 'C', 'B')
   ('A', 'C', 'D')
   ('A', 'D', 'B')
   ('A', 'D', 'C')
   ('B', 'A', 'C')
   ('B', 'A', 'D')
   ('B', 'C', 'A')
   ('B', 'C', 'D')
   ('B', 'D', 'A')
   ('B', 'D', 'C')
   ('C', 'A', 'B')
   ('C', 'A', 'D')
   ('C', 'B', 'A')
   ('C', 'B', 'D')
   ('C', 'D', 'A')
   ('C', 'D', 'B')
   ('D', 'A', 'B')
   ('D', 'A', 'C')
   ('D', 'B', 'A')
   ('D', 'B', 'C')
   ('D', 'C', 'A')
   ('D', 'C', 'B')
   '''
   ```

   ```python
   for i in itertools.permutations(range(3)):
       print(i)
   '''
   (0, 1, 2)
   (0, 2, 1)
   (1, 0, 2)
   (1, 2, 0)
   (2, 0, 1)
   (2, 1, 0)
   '''
   ```

3. combinations——组合

   ```python
   # 2是组合的长度
   for i in itertools.combinations("ABCD", 2):
       print(i)
   '''
   ('A', 'B')
   ('A', 'C')
   ('A', 'D')
   ('B', 'C')
   ('B', 'D')
   ('C', 'D')
   '''
   ```

   ```python
   for i in itertools.combinations(range(4), 3):
       print(i)
   '''
   (0, 1, 2)
   (0, 1, 3)
   (0, 2, 3)
   (1, 2, 3)
   '''
   ```

4. combinations_with_replacement——元素可重复组合

   ```python
   # 2是组合的长度
   for i in itertools.combinations_with_replacement('ABC', 2):
       print(i)
   '''
   ('A', 'A')
   ('A', 'B')
   ('A', 'C')
   ('B', 'B')
   ('B', 'C')
   ('C', 'C')
   '''
   ```

### 1.4.2 拉链

1. zip——短拉链

   ```python
   for i in zip("ABC", "012", "xyz"):
       print(i)
   '''
   ('A', '0', 'x')
   ('B', '1', 'y')
   ('C', '2', 'z')
   '''
   ```

   ```python
   # 长短不一时，执行到最短的对象处就停止
   for i in zip("ABC", "012345"):
       print(i)
   '''
   ('A', '0')
   ('B', '1')
   ('C', '2')
   '''
   ```

2. zip_longest——长拉链

   长度不一时，执行到最长的对象处，就停止。缺省元素用None 或指定字符代替

   ```python
   for i in itertools.zip_longest("ABC", "012345"):
       print(i)
   '''
   ('A', '0')
   ('B', '1')
   ('C', '2')
   (None, '3')
   (None, '4')
   (None, '5')
   '''
   ```

   ```python
   for i in itertools.zip_longest("ABC", "012345", fillvalue="?"):
       print(i)
   '''
   ('A', '0')
   ('B', '1')
   ('C', '2')
   ('?', '3')
   ('?', '4')
   ('?', '5')
   '''
   ```

### 1.4.3 无穷迭代器

1. count(start=0, step=1)——计数

   创建一个迭代器，它从start值开始，返回均匀间隔的值

   ```python
   for i in itertools.count(1, 1):
       print(i)
   ```

2. cycle(iterable)——循环

   创建一个迭代器，返回iterable中所有元素，无限重复

   ```python
   for i in itertools.cycle("ABC"):
       print(i)
   ```

3. repeat(object[, times])——重复

   创建一个迭代器，不断重读object。除非设定参数times，否则将无限重复

   ```python
   for i in itertools.repeat(10, 3):
       print(i)
   ```

### 1.4.4 其他

1. chain(iterable)——锁链

   把一组迭代对象串联起来，形成一个更大的迭代器

   ```python
   for i in itertools.chain("ABC", [1, 2, 3]):
       print(i)
   '''
   A
   B
   C
   1
   2
   3
   '''
   ```

2. enumerate(iterable, start=0)——枚举（Python内置）

   产生由两个元素组成的元组，结构是(index, item)，其中index从start开始，item从iterable中取

   ```python
   for i in enumerate("Python", start=2):
       print(i)
   '''
   (2, 'P')
   (3, 'y')
   (4, 't')
   (5, 'h')
   (6, 'o')
   (7, 'n')
   '''
   ```

3. groupby(iterable, key=None)——分组

   创建一个迭代器，按照key指定的方式，返回iterable中连续的键和组

   一般来说，要预先对数据进行排序

   key为None默认把连续重复元素分组

   ```python
   for key, group in itertools.groupby('AAAABBBBCCCDAAABB'):
       print(key, list(group))
   '''
   A ['A', 'A', 'A', 'A']
   B ['B', 'B', 'B', 'B']
   C ['C', 'C', 'C']
   D ['D']
   A ['A', 'A', 'A']
   B ['B', 'B']
   '''
   ```

   ```python
   animals = ["duck", "eagle", "cat", "giraffe", "bear", "bat", "dolphin", "shark", "lion"]
   animals.sort(key=len)
   print(animals)
   print("--------------------")
   for key, group in itertools.groupby(animals, key=len):
       print(key, list(group))
   '''
   ['cat', 'bat', 'duck', 'bear', 'lion', 'eagle', 'shark', 'giraffe', 'dolphin']
   --------------------
   3 ['cat', 'bat']
   4 ['duck', 'bear', 'lion']
   5 ['eagle', 'shark']
   7 ['giraffe', 'dolphin']
   '''
   ```

   ```python
   animals = ["duck", "eagle", "cat", "giraffe", "bear", "bat", "dolphin", "shark", "lion"]
   animals.sort(key=lambda x: x[0]) # 按首字母进行分组
   print(animals)
   print("-----------------------")
   for key, group in itertools.groupby(animals, key=lambda x: x[0]):
       print(key, list(group))
   '''
   ['bear', 'bat', 'cat', 'duck', 'dolphin', 'eagle', 'giraffe', 'lion', 'shark']
   -----------------------
   b ['bear', 'bat']
   c ['cat']
   d ['duck', 'dolphin']
   e ['eagle']
   g ['giraffe']
   l ['lion']
   s ['shark']
   '''
   ```

