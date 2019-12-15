<!--
 * @Author: Hiseh
 * @Date: 2019-12-12 18:10:23
 * @LastEditors: Hiseh
 * @LastEditTime: 2019-12-15 12:40:59
 * @Description: 密钥格式化
 -->
# 密钥格式化
[返回首页](../README.md)

这道题有一点难理解的地方是第一块长度可以少于`K`。如果先判断`S`去掉`-`后长度是否是`K`的整倍数，会很麻烦。所以我们换个思路，从后往前算，第一块算是余数，那就简单多了。
## Python
两行搞定。
```python
class Solution:
    def licenseKeyFormatting(self, S: str, K: int) -> str:
        # 反转S，方便从后往前算
        s_rev = S.upper().replace('-', '')[::-1]

        # 按着K的数量，依次用-拼出倒序的字符串，最后再反转
        return '-'.join([s_rev[i:i + K] for i in range(0, len(s_rev), K)])[::-1]
```
---

## C
题目里没有说明如何释放返回值，试错后才明白要调用者手动释放。
借用Python思路，用C重写。
```c
#include <stdlib.h>
#include <string.h>

char* licenseKeyFormatting(char* S, int K) {
    int result_len = 0;

    // 取得所有密钥字符，倒序存储
    int S_len = strlen(S);
    char tmp_arr[S_len + 1];
    for (int i = S_len - 1; i >= 0; i--) {
        if (S[i] != '-') {
            tmp_arr[result_len++] = S[i];
        }
    }
    tmp_arr[result_len] = '\0';

    // 计算-的个数
    int split_num =
        (--result_len) % K == 0 ? result_len / K - 1 : result_len / K;
    result_len += split_num + 1;

    // 在堆上申请内存
    char* result = (char*)malloc(result_len * sizeof(char));
    if (result != NULL) {
        for (int i = strlen(tmp_arr) - 1, j = 0; i >= 0; i--, j++) {
            result[j] = tmp_arr[i];
            if (i % K == 0 && i > 0) {
                result[++j] = '-';
            }
        }
    }
    return result;
}
```
[返回首页](../README.md)