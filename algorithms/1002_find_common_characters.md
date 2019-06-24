# 查找常用字符
[返回首页](../README.md)

## Python
使用Python库函数，一行搞定
```python
import collections
import functools

class Solution:
    def commonChars(self, A: List[str]) -> List[str]:
        '''
        参考官网文档Counter类的例子：两个Counter对象按位与，可以返回其交集。最后的结果就是所有字符串中都包含的公共字符。

        更易理解的写法：
        c = collections.Counter(A[0])
        for word in A:
            c &= collections.Counter(word)
        return list(c.elements()
        '''
        return list(functools.reduce(collections.Counter.__and__, map(collections.Counter, A)).elements())
```
---

## C
借用Python思路（这个思路类似用时间换空间，内存占用较少但cpu占用较多）
如果为了提高执行速度，可以用2个数组替代hash。
```c
/*
* 例 1: 28ms
*/
#include <stdbool.h>
#include <stdlib.h>
#include <string.h>
#include "uthash.h"

// 用hash表模拟Python中Counter对象
typedef struct {
    char charater[2];
    int num;
    UT_hash_handle hh;
} chars_h;
chars_h* chars = NULL;

char** commonChars(char** A, int ASize, int* returnSize) {
    // 初始化chars
    chars_h *item, *tmp = NULL;
    char a_0[2];
    a_0[1] = '\0';
    for (int i = 0; i < strlen(A[0]); i++) {
        a_0[0] = A[0][i];
        HASH_FIND_STR(chars, a_0, tmp);
        if (tmp == NULL) {
            tmp = (chars_h*)malloc(sizeof(chars_h));
            tmp->charater[0] = A[0][i];
            tmp->charater[1] = '\0';
            tmp->num = 1;
            HASH_ADD_STR(chars, charater, tmp);
        } else {
            tmp->num++;
        }
    }

    // 1、如果hash表中无当前字符，则删除hash中的元素
    // 2、如果hash表中有当前字符，则取最小交集
    for (int i = 0; i < ASize; i++) {
        tmp = NULL;
        item = NULL;
        HASH_ITER(hh, chars, item, tmp) {
            int counter = 0;
            for (int j = 0; j < strlen(A[i]); j++) {
                // 计数+1
                if (item->charater[0] == A[i][j]) {
                    counter++;
                }
            }

            if (counter == 0) {
                // 删掉item
                HASH_DEL(chars, item);
                free(item);
            } else {
                // 保存字母出现最少次数
                item->num = item->num < counter ? item->num : counter;
            }
        }
    }

    // 计算返回数组大小
    *returnSize = 0;
    for (item = chars; item != NULL; item = item->hh.next) {
        (*returnSize) += item->num;
    }

    // 调用者里记得释放
    char** result = (char**)malloc((*returnSize) * sizeof(char*));
    int result_i = 0;

    // 拼成返回数组
    HASH_ITER(hh, chars, item, tmp) {
        for (int i = 0; i < item->num; i++, result_i++) {
            result[result_i] = (char*)malloc(2 * sizeof(char));
            strcpy(result[result_i], item->charater);
        }

        HASH_DEL(chars, item);
        free(item);
    }
    return result;
}
```
```c
/*
* 例 2: 8ms
*/
void CharCounter(char* string, int* CounterArray) {
    // 统计string中每个字母出现次数，存入CounterArray中
    int index = 0;

    for (index = 0; index < strlen(string); index++)

        CounterArray[string[index] - 'a']++;
}

void CharCounterClear(int* CounterArray, int ArraySize) {
    // 把CounterArray数据清零
    int index = 0;

    for (index = 0; index < ArraySize; index++)
        CounterArray[index] = 0;
}

char** commonChars(char** A, int ASize, int* returnSize) {
    int CounterArraySize = 26;
    int AIndex = 0, CharIndex = 0;
    int returnArraySize = 0;
    int Counter_A[26] = {0}, Counter_B[26] = {0};

    // 统计A[0]中字母出现频率，存到Counter_A
    CharCounter(A[0], Counter_A);

    // 1 <= A.length <= 100
    char** returnArr = (char**)calloc(100, sizeof(char*));
    for (int i = 0; i < 100; i++) {
        // 因为要保存字符串数组
        returnArr[i] = (char*)calloc(2, sizeof(char));
    }

    //统计所有字符出现频率
    for (AIndex = 1; AIndex < ASize; AIndex++) {
        CharCounterClear(Counter_B, CounterArraySize);

        // 数清楚A[AIndex]中字符出现频率，存到Counter_B
        CharCounter(A[AIndex], Counter_B);

        // 将Counter_A字符出现频率记录成每个单词中最小值（最小就是0）
        for (CharIndex = 0; CharIndex < CounterArraySize; CharIndex++)
            Counter_A[CharIndex] = Counter_A[CharIndex] < Counter_B[CharIndex]
                                       ? Counter_A[CharIndex]
                                       : Counter_B[CharIndex];
    }

    // 拼成返回数组
    for (CharIndex = 0; CharIndex < CounterArraySize; CharIndex++) {
        int nCounter = 0;

        for (nCounter = 0; nCounter < Counter_A[CharIndex]; nCounter++) {
            char c[2];

            c[0] = CharIndex + 'a';
            c[1] = '\0';

            strcpy(returnArr[returnArraySize], c);
            returnArraySize++;
        }
    }

    *returnSize = returnArraySize;

    return returnArr;
}
```
[返回首页](../README.md)