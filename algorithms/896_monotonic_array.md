<!--
 * @Author: Hiseh
 * @Date: 2019-10-23 17:24:14
 * @LastEditors: Hiseh
 * @LastEditTime: 2019-10-24 17:38:44
 * @Description: 
 -->
# 单调数列 
[返回首页](../README.md)

这道题只用考虑三种情况：
1. 递增
2. 都一样
3. 递减

只需要遍历一次数组，时间复杂度Θ(n)。
## Python
```python
class Solution:
    def isMonotonic(self, A: list) -> bool:
        # 判断数组应该是何种情况
        if A[0] < A[-1]:
            increasing = 1
        elif A[0] == A[-1]:
            increasing = 0
        else:
            increasing = -1

        # 遍历检查
        for i, e in enumerate(A):
            try:
                if increasing == 1:
                    if e > A[i + 1]:
                        return False
                elif increasing == 0:
                    if e != A[i + 1]:
                        return False
                else:
                    if e < A[i + 1]:
                        return False
            except IndexError:
                pass
        return True
```
---

## C
用C重写Python代码即可。
```c
#include <stdbool.h>

bool isMonotonic(int* A, int ASize) {
    char increasing = 0;
    if (A[0] < A[ASize - 1]) {
        increasing = 1;
    } else if (A[0] == A[ASize - 1]) {
        increasing = 0;
    } else {
        increasing = -1;
    }

    for (int i = 0; i < ASize - 1; i++) {
        switch (increasing) {
            case 1:
                if (A[i] > A[i + 1]) {
                    return false;
                }
                break;
            case 0:
                if (A[i] != A[i + 1]) {
                    return false;
                }
                break;
            case -1:
                if (A[i] < A[i + 1]) {
                    return false;
                }
                break;

            default:
                break;
        }
    }
    return true;
}
```
[返回首页](../README.md)