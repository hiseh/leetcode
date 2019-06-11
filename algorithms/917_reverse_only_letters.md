# 仅仅反转字母 
[返回首页](../README.md)

## Python
我们知道，利用slice notation可以很方便地翻转字符串，唯一要注意的就是原位置保留非字母符号。
```python
class Solution:
    def reverseOnlyLetters(self, S: str) -> str:
        s_l = []
        not_alpha_l = {}

        for i, e in enumerate(S):
            if e.isalpha():
                s_l.append(e)
            else:
                # 记住非字母符号位置
                not_alpha_l[i] = e

        s_l = s_l[::-1]

        for i, c in not_alpha_l.items():
            # 反转完再把非字母符号插回去
            s_l.insert(i, c)

        return ''.join(s_l)

```
---

## C
C99没有变长数组的概念，如果借用python思路，需要用链表和哈希表，麻烦很多。此处用了一个讨巧的技巧。
```c
#include <ctype.h>

char* reverseOnlyLetters(char* S) {
    int size = strlen(S);
    for (int i = 0, j = size - 1; i < j; i++, j--) {
        while (i < j && !isalpha(S[i])) {
            i++;
        }
        while (i < j && !isalpha(S[j])) {
            j--;
        }
        if (!(i < j)) {
            break;
        }

        S[i] ^= S[j];
        S[j] ^= S[i];
        S[i] ^= S[j];
    }
    return S;
}

```
[返回首页](../README.md)