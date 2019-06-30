# Nim游戏
[返回首页](../README.md)

按题的意思，如果石头数是4的倍数，则你输，否则就赢了。非常简单
## Python
```python
class Solution:
    def canWinNim(self, n: int) -> bool:
        return n % 4 != 0
```
---

## C
```c
#include <stdbool.h>

bool canWinNim(int n) {
    return n % 4 != 0;
}
```
[返回首页](../README.md)