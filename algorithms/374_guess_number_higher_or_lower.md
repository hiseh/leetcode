# 猜数字大小
[返回首页](../README.md)

这道题的描述有点不好理解，其实就是有个guess函数，会根据输入数字返回`1/0/-1`三个状态，表示输入数字比目标值偏小/相等/偏大。
## Python
思路很简单，用二分法。
```python
class Solution(object):
    def guessNumber(self, n: int) -> int:
        low, high = 1, n
        while low < high:
            middle = (low + high) / 2
            low, high = ((middle, middle), (middle + 1, high), (low, middle - 1))[guess(middle)]
        return low
```
---

## C
用C重写Python代码即可。
```c
#include <stdlib.h>

int guessNumber(int n) {
    int low = 1;
    int hight = n;
    int middle = NULL;
    while (low < hight) {
        middle = (int)((low + hight) / 2);
        switch (guess(middle)) {
            case -1:
                hight = middle - 1;
                break;
            case 0:
                return middle;
                break;
            case 1:
                low = middle + 1;
                break;
            default:
                break;
        }
    }
}
```
[返回首页](../README.md)