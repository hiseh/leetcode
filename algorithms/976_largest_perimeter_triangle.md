# 三角形的最大周长
[返回首页](../README.md)

给定由一些正数（代表长度）组成的数组 A，返回由其中三个长度组成的、面积不为零的三角形的最大周长。

如果不能形成任何面积不为零的三角形，返回 0。

大家都记得三角形边长有个定理：两边长度之和一定大于第三边长度。就用这个定理来判断是否能组成三角形。
## Python
```python
class Solution:
    def largestPerimeter(self, A: List[int]) -> int:
        # 把数组倒序排列，原则上肯定越大的数组成三角形周长越长。
        A.sort(reverse=True)
        i = 0

        # 找到最大的能组成三角形的三个数
        # 1、有3个数；
        # 2、两边之和>第三边
        while i < len(A) - 2 and A[i] >= A[i + 1] + A[i + 2]:
            i += 1

        # 如果找到，返回三个数之和，否则返回0
        return A[i + 1] + A[i + 2] + A[i] if i < len(A) - 2 else 0

```
---

## C
思路同Python
```c
#include <stdlib.h>

int compare_ints(const void* a, const void* b) {
    return *(int*)b - *(int*)a;
}

int largestPerimeter(int* A, int ASize) {
    // 倒序排列
    // qsort就是简单的快排，对于最坏情况效率非常低，特殊情况可以考虑换成其它库，比如klib
    qsort(A, ASize, sizeof(int), compare_ints);

    int i = 0;
    while (i < ASize - 2 && A[i] >= A[i + 1] + A[i + 2]) {
        i++;
    }

    return i < ASize - 2 ? A[i + 1] + A[i + 2] + A[i] : 0;
}

```
[返回首页](../README.md)