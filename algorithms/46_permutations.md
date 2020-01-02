<!--
 * @Author: Hiseh
 * @Date: 2020-01-02 10:56:28
 * @LastEditors  : Hiseh
 * @LastEditTime : 2020-01-02 15:24:43
 * @Description: 全排列
 -->
# 全排列
[返回首页](../README.md)

题目很简单，就是返回*P<sub>n</sub><sup>n</sup>*。
## Python
用自带的`permutations`库实现。
```python
import itertools

class Solution:
    def permute(self, nums: list) -> list
        return list(itertools.permutations(nums,len(nums)))
```
---

## C
C标准库里没有现成的排列函数，需要自己实现。我觉得最好理解的思路用递归方式，遍历一个完全树，从上往下，每层确定前n个数字的排列顺序。具体讲解请参照[给定数字能组成的最大时间](./949_largest_time_for_given_digits.md#C)，本题其实就是问如何实现949题排列部分。
```c
// 照搬949题排列部分
#include <string.h>
#include <stdlib.h>

void swap(int* x, int* y) {
    int temp;
    temp = *x;
    *x = *y;
    *y = temp;
}

void permutation(int* a, int l, int r, int* count, int** ret, int numsSize, int** returnColumnSizes) {
    int i;
    if (l == r) {
        *(ret + *count) = malloc(sizeof(int) * numsSize);
        memcpy(*(ret + *count), a, numsSize * sizeof(int));
        *returnColumnSizes = realloc(*returnColumnSizes, sizeof(int) * (*(count) + 1));
        *(*returnColumnSizes + (*count)) = numsSize;
        (*count)++;

    } else {
        for (i = l; i <= r; i++) {
            swap((a + l), (a + i));
            permutation(a, l + 1, r, count, ret, numsSize, returnColumnSizes);
            swap((a + l), (a + i));
        }
    }
}
int** permute(int* nums, int numsSize, int* returnSize, int** returnColumnSizes) {
    int cnt = 0;
    *returnColumnSizes = malloc(sizeof(int) * 1);
    int** ret = malloc(sizeof(int*) * 1000);
    permutation(nums, 0, numsSize - 1, &cnt, ret, numsSize, returnColumnSizes);
    *returnSize = cnt;
    return ret;
}
```
[返回首页](../README.md)