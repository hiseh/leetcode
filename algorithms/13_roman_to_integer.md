# 罗马数字转阿拉伯数字
[返回首页](../README.md)

罗马数字有个特点，就是通常情况用加法表示数字，比如12写作`XII`，意思是`10+1+1`，35写作`XXXV`，意思是`10+10+10+5`。除了通常的加法表示外，还有6种减法表示方式：
- `I`可以放在`V`和`X`的左边，表示4和9，也就是`V-I`和`X-I`。
- `X`可以放在`L`和`C`的左边，表示40和90，也就是`L-X`和`C-X`。 
- `C`可以放在`D`和`M`的左边，表示400和900，也就是`D-C`和`M-C`。
## Python
```python
class Solution:
    def romanToInt(self, s: str) -> int:
        result = 0

        max_i = len(s)
        roman = {
            'I': 1,
            'V': 5,
            'X': 10,
            'L': 50,
            'C': 100,
            'D': 500,
            'M': 1000
        }

        # Python没有switch case语句，用dict+lambda代替
        # 遇到需要减法表示的数字时，将对应的数字转为负数
        roman_sub = {
            'I': lambda x: -roman['I'] if x in {'V', 'X'} else roman['I'],
            'X': lambda x: -roman['X'] if x in {'L', 'C'} else roman['X'],
            'C': lambda x: -roman['C'] if x in {'D', 'M'} else roman['C']
        }

        # 正常加法操作
        for i, v in enumerate(s):
            if (i < max_i - 1) and (v in {'I', 'X', 'C'}):
                result += roman_sub[v](s[i + 1])
            else:
                result += roman[v]

        return result
```
---

## C
思路与Python一致：
```c
#include <stdlib.h>
#include <string.h>

int intVal(char x) {
    int result = NULL;
    switch (x) {
        case 'I':
            result = 1;
            break;
        case 'V':
            result = 5;
            break;
        case 'X':
            result = 10;
            break;
        case 'L':
            result = 50;
            break;
        case 'C':
            result = 100;
            break;
        case 'D':
            result = 500;
            break;
        case 'M':
            result = 1000;
            break;
    }
    return result;
}

int romanSub(char int1, char int2) {
    int sign = 1;
    switch (int1) {
        case 'I':
            if (int2 == 'V' || int2 == 'X')
                sign = -1;
            break;
        case 'X':
            if (int2 == 'L' || int2 == 'C')
                sign = -1;
            break;
        case 'C':
            if (int2 == 'D' || int2 == 'M')
                sign = -1;
            break;
    }
    return sign * intVal(int1);
}

int romanToInt(char* s) {
    int i = 0;
    int max_i = strlen(s);
    int result = 0;
    while (*s != '\0') {
        if ((i < max_i - 1) && (*s == 'I' || *s == 'X' || *s == 'C')) {
            result += romanSub(*s, *(s + 1));
        } else {
            result += intVal(*s);
        }
        s++;
        i++;
    }
    return result;
}
```
[返回首页](../README.md)