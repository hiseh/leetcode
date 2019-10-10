<!--
 * @Author: Hiseh
 * @Date: 2019-10-09 10:24:36
 * @LastEditors: Hiseh
 * @LastEditTime: 2019-10-10 11:30:31
 * @Description: 
 -->
# 验证回文串
[返回首页](../README.md)

其实就是检查统一大小写，去掉非字母和数字之后的字符串是否是回文串，非常简单。
## Python
对于python3，正则比`string.translate`效率高（与py2正好相反）。
```python
import re


class Solution:
    def isPalindrome(self, s: str) -> bool:
        s = s.lower()

        # 因为只用到字母和数字，所以找出字母和数字后拼成新字符串比在原有字符串中删除非字母和数字效率高
        pal_s = ''.join(re.findall('[a-z0-9]+', s))
        return pal_s == pal_s[::-1]
```
---

## C
GNU默认用的`Perl-Compatible Regular Expression`正则库，在当前版本的leetcode里不太好用，因此不用正则，用循环实现。除了没用正则，其它地方采用Python思路。
```c
#include <ctype.h>
#include <stdbool.h>
#include <string.h>

bool isPalindrome(char* s) {
    if (strlen(s) == 0) {
        return true;
    }

    char res[strlen(s)];
    int res_i = 0;
    while (*s != '\0') {
        if (isalnum(*s)) {
            res[res_i++] = tolower(*s);
        }

        s++;
    }

    for (int i = 0; i < res_i / 2; i++) {
        if (res[i] != res[res_i - i - 1]) {
            return false;
        }
    }
    return true;
}
```
[返回首页](../README.md)