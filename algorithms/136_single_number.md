<!--
 * @Author: Hiseh
 * @Date: 2019-12-16 14:49:11
 * @LastEditors  : Hiseh
 * @LastEditTime : 2019-12-27 14:08:31
 * @Description: 
 -->
# 只出现一次的数字
[返回首页](../README.md)

题目要求**linear runtime complexity**和**without using extra memory**，首先考虑遍历数组同时在数组元素上操作，我能想到的是用*异或*位运算了：`a⊕a⊕b = b`
## Python
一行代码搞定。
```python
import functools


class Solution:
    def singleNumber(self, nums: list) -> int:
        return functools.reduce(lambda x, y: x ^ y, nums)
```
---

## C
用循环代替Python的`reduce`，其它思路一样。
```c
int singleNumber(int* nums, int numsSize) {
    int x = nums[0];
    for (int i = 1; i < numsSize; i++) {
        x ^= nums[i];
    }
    return x;
}
```
[返回首页](../README.md)