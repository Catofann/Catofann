---
layout: post
title: Python使用Matplotlib图表无法显示中文的问题
date: "2021-09-18"
tags: [Python, Matplotlib]
---



# Python使用Matplotlib图表无法显示中文的问题

如下图代码:

```python
import matplotlib.pyplot as plt
import matplotlib

input_values = [1, 2, 3, 4, 5]
squares = [1, 4, 9, 16, 25]
plt.style.use('seaborn')
fig, ax = plt.subplots()
ax.plot(squares)
ax.scatter(2, 4)
ax.plot(input_values)
# 设置图表标题并给坐标轴加上标签。
ax.set_title("平方数")
ax.set_xlabel("值")
ax.set_ylabel("值的平方")
ax.tick_params(axis='both', which='major', labelsize=14)
plt.show()
```

运行后发现中文无法显示：

![image-20210918161455685](https://pic.imgdb.cn/item/6145a34a2ab3f51d91776311.jpg)

参考[文章](https://zhuanlan.zhihu.com/p/104081310)，使用如下代码查询支持的中文字体：

```python
# 查询当前系统所有字体
from matplotlib.font_manager import FontManager
import subprocess

mpl_fonts = set(f.name for f in FontManager().ttflist)

print('all font list get from matplotlib.font_manager:')
for f in sorted(mpl_fonts):
    print('\t' + f)
```

在代码中添加`matplotlib.rc("font",family='KaiTi')`，如下图所示：

```python
import matplotlib.pyplot as plt
import matplotlib

matplotlib.rc("font",family='KaiTi')
input_values = [1, 2, 3, 4, 5]
squares = [1, 4, 9, 16, 25]
plt.style.use('seaborn')
fig, ax = plt.subplots()
ax.plot(squares)
ax.scatter(2, 4)
ax.plot(input_values)
# 设置图表标题并给坐标轴加上标签。
ax.set_title("平方数")
ax.set_xlabel("值")
ax.set_ylabel("值的平方")
ax.tick_params(axis='both', which='major', labelsize=14)
plt.show()
```

运行程序，发现图表仍然无法正常显示中文，原因是在设置字体后，又设置了主题，覆盖了字体设置，故而只需要将`matplotlib.rc("font",family='KaiTi')`语句放到主题设置语句`plt.style.use('seaborn')`之后即可，修改后代码如下：

```python
import matplotlib.pyplot as plt
import matplotlib

input_values = [1, 2, 3, 4, 5]
squares = [1, 4, 9, 16, 25]
plt.style.use('seaborn')
matplotlib.rc("font",family='KaiTi')
fig, ax = plt.subplots()
ax.plot(squares)
ax.scatter(2, 4)
ax.plot(input_values)
# 设置图表标题并给坐标轴加上标签。
ax.set_title("平方数")
ax.set_xlabel("值")
ax.set_ylabel("值的平方")
ax.tick_params(axis='both', which='major', labelsize=14)
plt.show()
```

运行后可见中文正常显示：

![image-20210918162236317](https://pic.imgdb.cn/item/6145a3232ab3f51d9177289c.jpg)