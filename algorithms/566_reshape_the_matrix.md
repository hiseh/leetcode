# 重构矩阵
[返回首页](../README.md)

用二维数组表示矩阵，也就是用新的行、列数重构数组，不改变原来的**行遍历顺序**

> **输入：** [[1,2,3],[4,5,6]]<br/>
> r = 3, c = 2
> 
> **输出：** [[1,2],[3,4],[5,6]]

## Python
itertools模块里有非常多操作迭代对象的函数，善用它们能大大减轻编程难度。
```python
import itertools

class Solution:
    def matrixReshape(self, nums: 'List[List[int]]', r: 'int',
                      c: 'int') -> 'List[List[int]]':

        # 检查新的行列数是否正确
        if r * c != len(nums) * len(nums[0]):
            return nums

        # chain函数可以把一组迭代对象串联起来，形成一个更大的迭代器
        # 这里是把二维数组转成一维数组
        plain_nums = itertools.chain(*nums)

        # 按列数依次取出一维数组中元素，转换成二维数组
        return [list(itertools.islice(plain_nums, c)) for _ in range(r)]
```
---

## C
思路同Python
```c
/**
 * Return an array of arrays of size *returnSize.
 * The sizes of the arrays are returned as *columnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */
#include <stdlib.h>

int** matrixReshape(int** nums,
                    int numsRowSize,
                    int numsColSize,
                    int r,
                    int c,
                    int** columnSizes,
                    int* returnSize) {
    
    // 检查新的行列数是否正确
    if (r * c != numsColSize * numsRowSize) {

        // 受题目描述约束，这里不能直接返回nums
        r = numsRowSize;
        c = numsColSize;
    }

    *returnSize = r;
    int** result = (int**)malloc(r * sizeof(int*));
    *columnSizes = (int*)malloc(r * sizeof(int));
    for (int i = 0; i < r; ++i) {
        result[i] = (int*)malloc(c * sizeof(int));
        (*columnSizes)[i] = c;
    }

    for (int i = 0; i < numsRowSize * numsColSize; ++i)
        // 这行很巧妙，矩阵转换
        result[i / c][i % c] = nums[i / numsColSize][i % numsColSize];

    return result;
}
```
[返回首页](../README.md)