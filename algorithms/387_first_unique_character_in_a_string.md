<!--
 * @Author: hiseh
 * @Date: 2019-07-23 16:52:57
 * @LastEditors: hiseh
 * @LastEditTime: 2019-07-24 11:51:55
 * @Description: 字符串中的第一个唯一字符
 -->

# 字符串中的第一个唯一字符
[返回首页](../README.md)

常规思路就是检查字符串中每个字符出现频率，然后取得频率为1的第一个字符位置。
## Python
调用自带的collections库，用最简单的常规思路解决问题。
```python
import collections

class Solution:
    def firstUniqChar(self, s: str) -> int:
        min_i = len(s)
        for k, v in collections.Counter(s).items():
            if v == 1:
                i = s.index(k)
                min_i = i if i < min_i else min_i

        return min_i if min_i < len(s) else -1
```
---

## C
既可以借用Python算法（效率较低），也可以换一种思考方式：这个题知道每个字母第一次出现的位置，以及该字母出现的频率即可，这样用两个循环也能解决问题，效率更高。
```c
// Python思路
#include <stdlib.h>
#include <string.h>
#include "uthash.h"

// 构建hash表
typedef struct {
    int key;
    int count;
    int idx;
    UT_hash_handle hh;
} counter_h;

counter_h* counter = NULL;

int firstUniqChar(char* s) {
    int idx = 0, tmp_i, s_len;
    counter_h *item, *tmp;

    // 遍历字符串，检查每个字母出现的频率及记一次的位置
    while (*s != '\0') {
        tmp_i = *s;
        HASH_FIND_INT(counter, &tmp_i, tmp);
        if (tmp == NULL) {
            tmp = (counter_h*)malloc(sizeof(counter_h));
            tmp->key = *s;
            tmp->idx = idx;
            tmp->count = 1;
            HASH_ADD_INT(counter, key, tmp);
        } else {
            tmp->count++;
        }
        idx++;
        s++;
    }
    s_len = idx;

    HASH_ITER(hh, counter, item, tmp) {
        if (item->count == 1) {
            idx = item->idx < idx ? item->idx : idx;
        }

        HASH_DEL(counter, item);
        free(item);
    }
    return idx < s_len ? idx : -1;
}
```
```c
// 巧算思路
#include <string.h>

int firstUniqChar(char* s) {
    int count[26] = {0};
    int len = strlen(s);

    // 查找每个字符出现频率，并把字符第一次出现的位置当作count数组下标
    for (int i = 0; i < len; i++) {
        count[(s[i] - 'a')]++;
    }

    // 返回第一个出现频率为1的坐标
    for (int i = 0; i < len; i++)
        if (count[(s[i] - 'a')] == 1)
            return i;
    return -1;
}
```
[返回首页](../README.md)