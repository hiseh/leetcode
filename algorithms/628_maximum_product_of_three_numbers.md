<!--
 * @Author: Hiseh
 * @Date: 2019-10-08 17:28:29
 * @LastEditors: Hiseh
 * @LastEditTime: 2019-10-08 20:53:57
 * @Description: 描述
 -->
# 三个数的最大乘积
[返回首页](../README.md)

这道题唯一费脑子的地方是要考虑负数的问题，如果有负数时，判断 *Min<sub>1</sub> × Min<sub>2</sub> × Max<sub>1</sub>* 是否大于 *Max<sub>1</sub> × Max<sub>2</sub> × Max<sub>3</sub>*

## Python
```python
import heapq
import functools


class Solution:
    def maximumProduct(self, nums: list) -> int:
        max_l = heapq.nlargest(3, nums)
        min_l = heapq.nsmallest(2, nums)

        max_m = functools.reduce(lambda x, y: x * y, max_l)
        min_m = min_l[0] * min_l[1]

        return max(min_m * max_l[0], max_m)
```
---

## C
Python的heapq库是堆排序，C没有默认的堆排序库，这里用快速排序代替（快速排序和堆排序的资料很好找，这里不再赘述）。其它思路同Python。
```c
#include <stdlib.h>

int compare(const void* p1, const void* p2) { return *(int*)p1 - *(int*)p2; }

int maximumProduct(int* nums, int numsSize) {
    int max_a[3], min_a[2];
    qsort(nums, numsSize, sizeof(int), compare);

    for (int i = 0; i < 2; i++) {
        max_a[i] = nums[numsSize - i - 1];
        min_a[i] = nums[i];
    }
    max_a[2] = nums[numsSize - 3];

    int max_m = 1;
    for (int i = 0; i < 3; i++) {
        max_m *= max_a[i];
    }

    int min_m = min_a[0] * min_a[1];

    return min_m * max_a[0] > max_m ? min_m * max_a[0] : max_m;
}
```
[返回首页](../README.md)