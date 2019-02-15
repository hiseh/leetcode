# 查找字符串数组中的最长公共前缀
[返回首页](../README.md)

## Python
Python自带zip函数，如果将可迭代的对象作为参数，会把对象中对应的元素打包成一个个tuple，然后返回由这些tuple组成的列表。
如果各个迭代器的元素个数不一致，则按着最短字符串长度打包。
```python
class Solution:
    def longestCommonPrefix(self, strs):
        """
        :type strs: List[str]
        :rtype: str
        """
        if not strs:
            return ''

        # 利用zip函数，按最短字符串长度，依次将相同位置的字符打包成多个tuple
        for i, e in enumerate(zip(*strs)):
            # 将tuple转成hash表，如果这个hash表中只有一个字符，说明当前位置的字符是公共前缀，否则就说明公共前缀已结束
            if len(set(e)) > 1:
                return strs[0][:i]
        else:
            # 如果打包的所有结果都是前缀，说明最短字符串就是公共前缀
            return min(strs)
```
---

## C
c语言没有zip函数，我的思路如下：

0. 假定有公共前缀，那么数组中每个字符串都会包含它，也就是前缀存在于所有字符串的前n位字符里
0. 如果前n位字符不是公共前缀，试着减小n，总会能找到这个前缀
0. 如果`n == 1`时依然没找到，则说明没有公共前缀
```c
#include <stdlib.h>
#include <string.h>

char* longestCommonPrefix(char** strs, int strsSize) {
    if (strsSize == 0) {
        return "";
    }

    // 假定第一个字符串是公共前缀
    char* prefix = strs[0];
    for (int i = 1; i < strsSize; i++) {
        // 检查当前字符串是否包含当前的公共前缀
        char* found = strstr(strs[i], prefix);

        // 如果不包含或第一个字符不相同，说明这个公共前缀太长了
        if (found == NULL || found > strs[i]) {
            // 循环缩短它，直到在当前字符串中从0开始包含了公共前缀
            while (strstr(strs[i], prefix) != strs[i]) {
                // 没找到公共前缀
                if (strlen(prefix) == 1) {
                    return "";
                }

                // 缩短公共前缀
                char* tmp = (char*)malloc(sizeof(char) * (strlen(prefix) - 1));
                strncpy(tmp, prefix, strlen(prefix) - 1);
                prefix = tmp;
            }
        }
    }
    return prefix;
}
```
[返回首页](../README.md)