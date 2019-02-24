# 回文数
[返回首页](../README.md)

判断一个整数是否是回文数。
## Python
考虑到Python有强大的的**slice notation**，当然优先使用slice notation了，转成字符串，一行搞定。

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        return str(x) == str(x)[::-1]
```
---

## C
C没有那么强大的列表操作语法糖，最直接的方式可以通过检查反转数字数字是否等于原始数字来判断。这么做有一个风险就是如果反转数字过大会造成内存溢出，还要增加额外的检查代码。

因此我们换个思路，如果这个整数是回文数，那么它后半部分反转后应该等于前半部分：
```c
#include <stdbool.h>

bool isPalindrome(int x) {
    // 如果是负数或者0结尾的正整数，那么肯定不是回文数
    if (x < 0 || (x % 10 == 0 && x != 0))
        return false;

    int reverseNum = 0;

    // 反转x，如果剩余x <= reverseNum，说明x已经反转了后半部分
    while (x > reverseNum) {
        reverseNum = reverseNum * 10 + x % 10;
        x /= 10;
    }

    // 如果x有奇数个数字，reverseNum会带上中间的数字，所以比较时要去掉它
    return x == reverseNum || x == reverseNum / 10;
}
```
[返回首页](../README.md)