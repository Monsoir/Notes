# Python 日期时间相关

- [time 模块](#time-模块)
    - [time.time()](#timetime)
    - [time.sleep()](#timesleep)
- [数字的四舍五入](#数字的四舍五入)
- [datetime 模块](#datetime-模块)
    - [获取当前的日期和时间](#获取当前的日期和时间)
    - [通过日期时间元素创建 datetime](#通过日期时间元素创建-datetime)
    - [通过时间戳创建 datetime](#通过时间戳创建-datetime)
    - [大小比较](#大小比较)
    - [timedelta 数据类型](#timedelta-数据类型)
    - [将 datetime 对象转换为格式化字符串](#将-datetime-对象转换为格式化字符串)
    - [将字符串转换成 datetime 对象](#将字符串转换成-datetime-对象)

## time 模块

读取系统时钟的当前时间

### time.time()

返回协调世界时间，1970 年 1 月 1 日 0 点，是一个浮点数

### time.sleep()

- 可传入要暂停的秒数作为参数
- 将会发生阻塞

---

对于一段长时间的睡眠，应使用循环睡眠一秒的方式进行

例如，睡眠 30 秒

```py
import time

time.sleep(30)
```

这种方式的睡眠，在睡眠过程中，将会长期阻塞进程，用户的键盘中断也不会起作用

```py
import time

for i in range(30):
    time.sleep(1)
```

这种方式的睡眠，用户的键盘中断是可以起作用的，不会浪费 CPU 处理周期来检查时间

## 数字的四舍五入

### round 函数

传入的参数

- 要四舍五入的数字 
- 小数点后精确到的位数

> 若省略第二个参数，则会返回最接近的整数

## datetime 模块

- 提供更强大的日期运算功能
- 有自己的数据类型

### 获取当前的日期和时间

返回的结果包含了当前时刻的年、月、日、时、分、秒和微秒的属性

```py
import datetime
datetime.datetime.now() # 略显啰嗦
```

### 通过日期时间元素创建 datetime

可以向 `datetime.datetime()` 传入年、月、日、时、分、秒的整数，获得特定时刻的 datetime 对象

### 通过时间戳创建 datetime

```py
import datetime
datetime.datetime.fromtimestamp(aTimeStamp)
```

### 大小比较

datetime 对象是可以直接比较大小，即可以使用 `==`, `>`, `<`, `!=` 进行大小比较

### timedelta 数据类型

datetime 提供的 timedelta 数据类型代表的是一段时间

---

timedelta 支持的关键字参数

- weeks
- days
- hours
- minutes
- seconds
- milliseconds
- microseconds

> 不支持 month 和 year 参数，是因为这两个参数依赖于特定的月份和年份

---

一个 timedelta 可以获取的属性

- days
- seconds
- microseconds

> 因此，timedelta 总是以天，秒，微秒来描述一段时间

---

timedelta 可以与 datetime 进行算术运算，如计算出 1000 天后的日期

```py
import datetime

dt = datetime.datetime.now()
thousandDays = datetime.deltatime(days=1000)
dt + thonsandsDays
```

类似这种的日期运算，Python 会为我们解决每个月有多少天，闰年因素的问题

> timedelta 也可以于整数与浮点数进行乘除运算

### 将 datetime 对象转换为格式化字符串

使用 `strftime()` 方法，可以将 datetime 对象显示为格式化字符串

> `strftime()` 中的 f 为 format 的意思
>
> 注意 `strftime()` 是方法，而不是函数，意味着需要在 datetime 对象进行调用

具体的格式化命令可以参考：

- [👉 这里](https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior)
- [👉 还有这里](http://strftime.org)

### 将字符串转换成 datetime 对象

使用 `strptime()` 函数，注意是函数

> `strptime()` 中的 p 为 parse 的意思

转换的占位命令与 `strftime()` 一样，可以参考 [👉 这里](https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior)


