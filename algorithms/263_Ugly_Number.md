# 判断是否为丑数
[返回首页](../README.md)

所谓丑数，就是只包含质因数 2, 3, 5的**正整数**。
## Python
解决思路非常直接，就是依次判断是否能被2/3/5整除。
```python
class Solution:
    def isUgly(self, num: int) -> bool:
        if num <2:
            return num > 0

        while num > 1:
            if (num / 2 == 1) or (num / 3 == 1) or (num / 5 == 1):
                return True
            if num % 2 == 0:
                num /= 2
            elif num % 3 == 0:
                num /= 3
            elif num % 5 == 0:
                num /= 5
            else:
                return False
```
---

## C
思路类似Python，稍微做了点微调。
```c
#include <stdbool.h>

bool isUgly(int num) {
    if (num < 2) {
        return (num > 0);
    }

    while (num % 2 == 0) {
        num /= 2;
    }

    while (num % 5 == 0) {
        num /= 5;
    }

    while (num % 3 == 0) {
        num /= 3;
    }
    return (num == 1);
}
```
[返回首页](../README.md)