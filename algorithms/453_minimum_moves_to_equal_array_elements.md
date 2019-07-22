<!--
 * @Author: hiseh
 * @Date: 2019-07-22 15:35:41
 * @LastEditors: hiseh
 * @LastEditTime: 2019-07-22 16:39:00
 * @Description: 最小移动次数使数组元素相等
 -->
# 最小移动次数使数组元素相等
[返回首页](../README.md)

倒过来考虑比较直观，如果给一个数组`[4, 4, 4]`，要移动几次才能变成`[1, 2, 3]`？是不是移动`(3 - 1) + (2 - 1) = 3`次，也就是移动*Σ(a<sub>1</sub> - a<sub>min</sub>) + (a<sub>2</sub> - a<sub>min</sub>) + ... + (a<sub>n</sub> - a<sub>min</sub>)*

## Python
```python
class Solution:
    def minMoves(self, nums: list[int]) -> int:
        return sum(nums) - min(nums) * len(nums)
```
---

## C
思路类似Python。可以看到，如果编程风格为pythonic的，并且只用cpython中内置函数，python效率也不算低。
```c
int minMoves(int* nums, int numsSize) {
    long sum = 0;
    long min = nums[0];
    for (int i = 0; i < numsSize; i++) {
        min = min < nums[i] ? min : nums[i];
        sum += nums[i];
    }

    return sum - min * numsSize;
}
```
[返回首页](../README.md)