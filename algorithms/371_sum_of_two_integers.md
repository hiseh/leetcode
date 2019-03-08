# 计算两个整数的和
[返回首页](../README.md)

不能使用`+`和`-`，计算两个整数的和。

## Python
不用+/-求和，对Python还叫事吗？
```python
class Solution:
    def getSum(self, a: int, b: int) -> int:
        return sum((a, b))
```
---

## C
C稍微麻烦点，可以用位运算实现
```c
int getSum(int a, int b) {

    // 直到carry == 0
    while (b != 0) {

        // carry为a和b公共位
        int carry = a & b;

        // 不带进位相加，公共位少了进位
        a = a ^ b;
        
        // 把carry进位补上，赋给b
        b = carry << 1;
    }
    return a;
}
```
[返回首页](../README.md)