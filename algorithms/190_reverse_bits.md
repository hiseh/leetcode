# 反转无符号32位二进制数
[返回首页](../README.md)

## Python
无符号数……太简单了，用slice notation一行代码搞定。
```python
int('{0:032b}'.format(n)[::-1], base=2)
```
---

## C
c复杂一点，我只想到用循环遍历一遍做反转。
```c
#include <stdint.h>

uint32_t reverseBits(uint32_t n) {
    uint32_t result = 0;
    // 遍历这个数字
    // 从第2次循环开始将输入值右移1，去掉最低位
    for (int i = 0; i < 32; i++, n >>= 1) {
        // result空出最低位
        result <<= 1;
        // 将输入值最低位补到result的最低位
        result |= n & 1;
    }
    return result;
}
```
[返回首页](../README.md)