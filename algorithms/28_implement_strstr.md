# 实现strStr()
[返回首页](../README.md)
strstr是C标准库函数，最新版GCC用的KMP算法，效率很高，我这里用了暴力破解效率是Θ(m*n)，结果C的效率还不如Python高。
## Python

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        for i in range(len(haystack) - len(needle) + 1):
            if haystack[i:i + len(needle)] == needle:
                return i
        return -1
```
---

## C
标准C库里`strstr`函数返回开始位置，稍微修改一下就能满足题目要求。这是1990年gcc代码，这套代码思路清晰，易于理解，虽然效率不高但节省内存，运行起来占用7.1MB内存。
```c

#include <string.h>

int strStr(char* haystack, char* needle) {
    int result = -1;

    if (strlen(needle) == 0) {
        return ++result;
    } else if (strlen(haystack) == 0) {
        return result;
    }

    char c, sc;
    size_t len;
    if ((c = *needle++) != 0) {
        len = strlen(needle);

        // 找到needle第一个字母开始位置后，然后检查haystack是不是包含了needle
        do {

            // 查找needle第一个字母开始位置
            do {
                if ((sc = *haystack++) == 0) {
                    // haystack到结束，说明没找到
                    return -1;
                }
                result++;
            } while (sc != c);
        } while (strncmp(haystack, needle, len) != 0);
        haystack--;
    }
    return result;
}
```
[返回首页](../README.md)