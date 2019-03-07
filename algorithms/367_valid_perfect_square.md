# 检查是否是完全平方数
这道题唯一费脑筋的地方是不允许使用内置sqrt库函数。我们第一时间想到的是用牛顿法开平方，可以用迭代也可以用递归实现。

[返回首页](../README.md)

## Python
```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        r = num
        while r * r > num:
            r = (r + num / r) // 2
        return r * r == num
```
---

## C
思路与Python一样，换下语法就可以。
```c
#include <stdbool.h>

bool isPerfectSquare(int num) {
    long r = num;
    while (r * r > num) {
        r = (r + num / r) / 2;
    }
    return r * r == num;
}
```
[返回首页](../README.md)