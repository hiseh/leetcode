# 完美数
[返回首页](../README.md)

思路有点类似确定一个数是不是质数，依然是遍历所有`1～给定数平方根`的数字，检查是否是因数。另外只有6/8结尾才可能是完美数（具体请参见欧几里得的完美数公式：如果2^p-1质数，那么（2^p-1）x 2^（p-1）便是一个完全数。），所以前面加点约束条件，可以提高效率。
## Python
一行代码搞定。
```python
import math


class Solution:
    def checkPerfectNumber(self, num: int) -> bool:
        if num % 10 not in {6, 8} or num < 2:
                return False

        return sum([sum([e, num // e]) for e in filter(lambda e: num % e == 0, range(2, math.floor(math.sqrt(num) + 1)))]) + 1 == num

```
---

## C
思路同Python，多写几行代码的事。
```c
#include <math.h>
#include <stdbool.h>

bool checkPerfectNumber(int num) {
    if ((num & 1 == 1) || (num < 2)) {
        return false;
    }

    int sum_factor = 1;
    for (int i = 2; i < floor(sqrt(num) + 1); i++) {
        if (num % i == 0) {
            sum_factor += i;
            sum_factor += num / i;
        }
    }
    return sum_factor == num;
}
```
[返回首页](../README.md)