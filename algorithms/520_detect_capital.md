# 检测大写字母
[返回首页](../README.md)

## Python
利用Python库函数，一行代码搞定
```python
class Solution:
    def detectCapitalUse(self, word: str) -> bool:
        # 1、全部大写
        # 2、首字母大写
        # 3、全部小写
        return word.isupper() or word.istitle() or word.islower()
```
---

## C
其实用正则很方便实现，可惜C标准库里没有正则。leetcode用gcc8.2，有时间可以查一下。

这里借用python的思路，用三个判断来检查。
```c
#include <ctype.h>
#include <stdbool.h>
#include <string.h>

bool detectCapitalUse(char* word) {
    // 因为后面取第1、2个字符前没有判断字符串长度，所以这里统一做一次判断
    if (strlen(word) == 0 || strlen(word) == 1) {
        return true;
    }

    // 分别取第1和第2个字符
    char first_char = *(word++);
    char sec_char = *(word++);

    bool all_upper = false;

    if (isupper(first_char) && isupper(sec_char)) {
        //都要大写
        all_upper = true;
    } else if (islower(first_char) && isupper(sec_char)) {
        return false;
    }

    // 遍历检查
    while (*word != '\0') {
        if (all_upper && islower(*word)) {
            return false;
        }

        if (!all_upper && isupper(*word)) {
            return false;
        }

        word++;
    }
    return true;
}
```
[返回首页](../README.md)