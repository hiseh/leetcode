# 按奇偶排序数组 II
[返回首页](../README.md)

给定一个非负整数数组 A， A 中一半整数是奇数，一半整数是偶数。

对数组进行排序，以便当 A[i] 为奇数时，i 也是奇数；当 A[i] 为偶数时， i 也是偶数。

> **输入:** [4,2,5,7]
> **输出:** [4,5,2,7]

## Python
使用zip和itertools，轻松搞定。
```python
import itertools

class Solution:
    def sortArrayByParityII(self, A: List[int]) -> List[int]:
        # 筛选出奇偶数组
        even_i = filter(lambda e: e & 1 == 0, A)
        odd_i = filter(lambda e: e & 1 == 1, A)

        # 把两个数组拼成一个数组
        return list(itertools.chain.from_iterable(zip(even_i, odd_i)))
```
---

## C
思路类似Python，单C可没有这么好用的库函数。
```c
#include <stdlib.h>
#include <string.h>

int* sortArrayByParityII(int* A, int ASize, int* returnSize) {
    // 奇偶元素下标，各自+2插着存
    int evenI = 0, oddI = 1;
    int* result = NULL;

    // 调用者记得要释放
    // 顺便将ASize赋值给returnSize，这两个数肯定相同
    result = (int*)malloc(sizeof(int) * (*returnSize = ASize));
    if (result != NULL) {
        for (int i = 0; i < ASize; i++) {
            if (A[i] % 2 == 0) {
                result[evenI] = A[i];
                evenI += 2;
            } else {
                result[oddI] = A[i];
                oddI += 2;
            }
        }
    }
    return result;
}
```
[返回首页](../README.md)