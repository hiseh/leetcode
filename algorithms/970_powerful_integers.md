<!--
 * @Author: Hiseh
 * @Date: 2019-12-16 14:49:11
 * @LastEditors  : Hiseh
 * @LastEditTime : 2019-12-27 21:03:48
 * @Description: 
 -->
# 强整数
[返回首页](../README.md)

强整数定义不难理解，解题思路就是找到所有指数可能组合，显然可以用笛卡尔积实现。
## Python
用python自带的库函数，3行代码搞定
```python
import math
import itertools


class Solution:
    def powerfulIntegers(self, x: int, y: int, bound: int) -> list[int]:
        # x指数范围，减少计算量
        x_log = 1 if x == 1 else int(math.log(bound, x)) + 1
        同理，y指数范围
        y_log = 1 if y == 1 else int(math.log(bound, y)) + 1

        # 1、生成0～x_log和0～y_log两个集合的笛卡尔积
        # 2、取出所有符合强整数定义的指数组合
        # 3、用hash保证结果唯一
        return set(map(lambda e: x**e[0] + y**e[1], filter(lambda e: x**e[0] + y**e[1] <= bound, itertools.product(range(x_log), range(y_log)))))
```
---

## C
用Python思路，自己实现笛卡尔积
```c
```
[返回首页](../README.md)