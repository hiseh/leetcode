# 特殊等价字符串组
[返回首页](../README.md)

题目描述比较难理解，其实意思是：如果两个字符串偶数位字符排序后相同，奇数位置字符排序后也相同，则为等价字符串。
## Python
采用上述思路，一行代码搞定。
```python
class Solution:
    def numSpecialEquivGroups(self, A: List[str]) -> int:
        # 1、把偶数字符和奇数字符排序后拼成一个新字符串
        # 2、返回新字符串的数量
        return len(set(map(lambda e: '{}{}'.format(''.join(sorted(e[::2])), ''.join(sorted(e[1::2]))), A)))
```
---

## C
借用Python思路，用C重写。
```c
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "uthash.h"

typedef struct {
    const char* key;
    UT_hash_handle hh;
} equiv_group_h;

equiv_group_h* equiv_group = NULL;

static inline int odd_num(int word_len) {
    // 奇数位字符数量
    return (int)ceil(word_len / 2);
}
static inline int even_num(int word_len) {
    // 偶数位字符数量
    return (int)floor(word_len / 2);
}

int compare_ints(const void* a, const void* b) {
    // 倒序排序
    return *(char*)b - *(char*)a;
}

int numSpecialEquivGroups(char** A, int ASize) {
    int result = 0;
    equiv_group_h *tmp = NULL, *curr = NULL;

    for (int i = 0; i < ASize; i++) {
        int word_len = strlen(A[i]);
        int odd_i = 0;
        int even_i = 0;
        char odd[odd_num(word_len)], even[even_num(word_len)];

        // 分别填充奇偶字符串
        for (int j = 0; j < word_len; j++) {
            if ((j & 1) == 1) {
                odd[odd_i++] = A[i][j];
            } else {
                even[even_i++] = A[i][j];
            }
        }

        // 排序奇偶字符串
        qsort(odd, odd_i, sizeof(char), compare_ints);
        qsort(even, even_i, sizeof(char), compare_ints);

        // 把排好序的奇偶字符串放到hash里
        char* key = (char*)malloc(sizeof(char) * word_len);
        sprintf(key, "%s%s", even, odd);
        HASH_FIND_STR(equiv_group, key, tmp);
        if (tmp == NULL) {
            tmp = (equiv_group_h*)malloc(sizeof(equiv_group_h));
            tmp->key = key;
            HASH_ADD_KEYPTR(hh, equiv_group, tmp->key, strlen(tmp->key), tmp);
            result++;
        }
    }

    HASH_ITER(hh, equiv_group, curr, tmp) {
        HASH_DEL(equiv_group, curr);
        free(curr);
    }
    return result;
}
```
[返回首页](../README.md)