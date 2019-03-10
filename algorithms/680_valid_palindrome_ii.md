# 验证回文字符串 II 
[返回首页](../README.md)

给定一个非空字符串 s，**最多**删除一个字符。判断是否能成为回文字符串。
## Python
```python
class Solution:
    def validPalindrome(self, s: str) -> bool:
        def is_pali_range(i, j):
            return all(s[k] == s[j - k + i] for k in range(i, j))

        for i in range(len(s) // i):
            # 检查对称的元素是否相等
            if s[i] != s[~i]:
                j = len(s) - 1 - i
                # 分别尝试去掉左右边界元素，检查边界内元素是否是回文
                return is_pali_range(i + 1, j) or is_pali_range(i, j - 1)
        return True
```
---

## C
思路同Python。写C时特怀念Python有那么多好用的库
```c
#include <stdbool.h>
#include <string.h>

bool is_pali_range(int i, int j, char* s) {
    bool result = true;
    for (int k = i; k < j; k++) {
        if (s[k] != s[j - k + i]) {
            result = false;
            break;
        }
    }
    return result;
}

bool validPalindrome(char* s) {
    int max_n = strlen(s);
    for (int i = 0; i < max_n / 2; i++) {
        if (s[i] != s[max_n - i - 1]) {
            int j = max_n - i - 1;
            return is_pali_range(i + 1, j, s) || is_pali_range(i, j - 1, s);
        }
    }
    return true;
}
```
[返回首页](../README.md)