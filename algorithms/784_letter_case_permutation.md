# 字母大小写全排列 
[返回首页](../README.md)

给定一个字符串`S`，通过将字符串`S`中的每个字母转变大小写，我们可以获得一个新的字符串。返回所有可能得到的字符串集合。
> **输入:** S = "a1b2"
> **输出:** ["a1b2", "a1B2", "A1b2", "A1B2"]
> 
> **输入:** S = "3z4"
> **输出:** ["3z4", "3Z4"]
>
> **输入:** S = "12345"
> **输出:** ["12345"]

如果将每个字母可区分成大/小写形式，位置不变，最后组成所有的可能不就是笛卡尔积吗？最后把生成的每个笛卡尔积结果拼成字符串返回即可。
## Python
时间复杂度O(n!)
```python
import itertools

class Solution:
    def letterCasePermutation_1(self, S: str) -> list[str]:
        # 如果是字母，转成一个纯大写，一个纯小写的二维集合，否则就是本身
        pattern = [(c.upper(), c.lower()) if c.isalpha() else c for c in S]

        # 计算笛卡尔积，并把每个结果都拼成字符串，组成数组返回
        return [''.join(c) for c in itertools.product(*pattern)
```
---

## C
总体思路与Python一致，都是用笛卡尔积实现。但C可没现成的函数，考虑到笛卡尔积概念就是多个集合所有可能有序对，如果所有集合都是只有1个元素，那么笛卡尔积也只有一个结果（**可能性**嘛），看到这，除了用递归，我也想不出别的办法了。

瞅瞅吧，2行Python能搞定的，C得多啰嗦。
```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
#include <ctype.h>
#include <stdlib.h>
#include <string.h>

void cartProduct(char** pattern,
                 int* patternLengths,
                 char* currentSet,
                 int patternSize,
                 int times,
                 char** result,
                 int* resultI) {

    // 说明已经是唯一结果了
    if (times == patternSize) {
        // 记得用拷贝的结果数据
        char* resultStr = (char*)malloc(patternSize * sizeof(char));
        strcpy(resultStr, currentSet);

        // 把结果存到二维数组
        result[(*resultI)++] = resultStr;
        return;
    } else {

        // 遍历大小写集合
        for (int i = 0; i < patternLengths[times]; i++) {

            // 依次尝试各种有序组合
            currentSet[times] = pattern[times][i];
            cartProduct(pattern, patternLengths, currentSet, patternSize,
                        times + 1, result, resultI);
        }
    }
}
char** letterCasePermutation(char* S, int* returnSize) {
    int SSize = strlen(S);

    // 申请全局变量
    char** pattern = (char**)malloc(SSize * sizeof(char*));
    int* patternLengths = (int*)malloc(SSize * sizeof(int));
    char* currentSet = (char*)malloc(sizeof(char) * (SSize + 1));

    char* holder;
    *returnSize = 1;
    for (int i = 0; S[i] != '\0'; i++) {
        if (isalpha(S[i]) > 0) {
            holder = (char*)malloc(2 * sizeof(char));
            holder[0] = tolower(S[i]);
            holder[1] = toupper(S[i]);

            // 笛卡尔积数量，每多一个字母，就翻倍
            *returnSize *= 2;
        } else {
            holder = (char*)malloc(sizeof(char));
            *holder = S[i];
        }
        patternLengths[i] = strlen(holder);
        pattern[i] = holder;
    }

    // 最终结果，调用者记得要释放
    char** result = (char**)malloc((*returnSize) * sizeof(char*));
    int resultI = 0;
    cartProduct(pattern, patternLengths, currentSet, SSize, 0, result,
                &resultI);

    for (int i = 0; i < SSize; i++) {
        free(pattern[i]);
    }
    free(pattern);
    free(patternLengths);
    free(currentSet);
    return result;
}
```
[返回首页](../README.md)