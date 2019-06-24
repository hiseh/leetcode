# 区域和检索 - 数组不可变
[返回首页](../README.md)

## Python
不考虑性能的话，Python一行搞定。而且这一行内存占用是Θ(1)，算是省内存非CPU吧。
```python
class NumArray:
    def __init__(self, nums: list):
        self.nums = nums

    def sumRange(self, i: int, j: int) -> int:
        return sum(self.nums[i: j + 1])
```
---

## C
可以直接借用Python思路，也可以优化下效率，否则C会比Python还慢（毕竟Python的`sum`函数可是用寄存器存储的）
```c
#include <stdlib.h>

typedef struct {
    int size;
    int* sums;
} NumArray;

NumArray* numArrayCreate(int* nums, int numsSize) {
    NumArray* num_arr = (NumArray*)malloc(sizeof(NumArray));
    num_arr->size = numsSize;
    if (numsSize > 0) {
        num_arr->sums = (int*)malloc(numsSize * sizeof(int));
        num_arr->sums[0] = nums[0];
        for (int i = 1; i < numsSize; i++) {
            num_arr->sums[i] = nums[i] + num_arr->sums[i - 1];
        }
    }
    return num_arr;
}

int numArraySumRange(NumArray* obj, int i, int j) {
    if (obj->size == 0) {
        return 0;
    }

    // 因为sums里保存了从0～j的元素和，所以这里用sum[j]-sum[i-1]计算从i~j的元素和
    return i == 0 ? obj->sums[j] : obj->sums[j] - obj->sums[i - 1];
}

void numArrayFree(NumArray* obj) {
    if (obj->size > 0) {
        free(obj->sums);
    }
    free(obj);
}

/**
 * Your NumArray struct will be instantiated and called as such:
 * NumArray* obj = numArrayCreate(nums, numsSize);
 * int param_1 = numArraySumRange(obj, i, j);

 * numArrayFree(obj);
*/

```
[返回首页](../README.md)