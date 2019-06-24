# 汇总区间
[返回首页](../README.md)<br>

给定一个无重复元素的有序整数数组，返回数组区间范围的汇总。

> **输入:** [0,1,2,4,5,7]<br/>
> **输出:** ["0->2","4->5","7"]<br/>
> **解释:** 0,1,2 可组成一个连续的区间; 4,5 可组成一个连续的区间。
## Python
Python中list在内存中是一个指针数组，对list的引用实际是引用它的地址，利用这个特性可以先引用，后修改list内容。
```python
class Solution:
    def summaryRanges(self, nums: list[int]) -> list[str]:

        # tmp存储连续元素
        result, tmp = [], []
        for e in nums:
            
            # 说明当前元素是第一个非连续元素
            if e - 1 not in tmp:

                # 清空tmp，同时分配一块新的内存
                tmp = []

                # 插入新的tmp
                result.append(tmp)
            
            # 如果tmp是空的，那么把当前元素插入第0位，否则插入第1位
            tmp[1:] = [e]

        # 格式化输出
        return ['->'.join(map(str, e)) for e in result]
```
---

## C
C里虽然对数组引用也是通过指针引用的，但C中内存不如Python灵活，如果手动申请（通过`malloc`/`calloc`/`realloc`），那么result中插入的tmp对应内存地址也是指定的，没办法动态绑定。如果自动申请（通过数组方式），则需要事先知道tmp长度，当然可以指定tmp长度与nums长度一样，这么做会浪费太多内存空间，最坏情况会浪费掉4*(n^2-n)字节。

因此这里用了常规的遍历+单向链表机制实现。
```c
#include <math.h>
#include <stdio.h>
#include <stdlib.h>

/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
typedef struct Result {
    char* smr;
    struct Result* next;
} Result;

// 与常规方式不同，这里要把新元素加到链表末尾
Result* append(Result* head, Result* element) {
    Result* cursor = head;
    while (cursor->next != NULL) {
        cursor = cursor->next;
    }
    cursor->next = element;
    return head;
}

// 格式化输出
char* formatResult(int l, int r) {
    // resultStr需要调用者中释放
    char* resultStr = NULL;
    if (l == r) {
        // log10返回其对10的指数，所以对于十进制数字，常用对数+1就是这个数字整数位的位数（长度）
        int size = l == 0 ? 1 : floor(log10f(l) + 1);
        resultStr = (char*)malloc(sizeof(char) * size);
        sprintf(resultStr, "%d", l);
    } else {
        int lsize = l == 0 ? 1 : floor(log10f(l) + 1);
        int rsize = r == 0 ? 1 : floor(log10f(r) + 1);
        resultStr = (char*)malloc(sizeof(char) * (lsize + rsize + 2));
        sprintf(resultStr, "%d->%d", l, r);
    }
    return resultStr;
}
char** summaryRanges(int* nums, int numsSize, int* returnSize) {
    // 这个单向列表向后延展，第一位留空
    Result* head = (Result*)malloc(sizeof(Result));
    head->smr = NULL;
    head->next = NULL;

    int start = 0;
    int i = 0;
    *returnSize = 0;
    for (; i < numsSize - 1; i++) {
        // 如果连续两个元素之差不是1，说明后面那个元素是第一个非连续元素
        if (nums[i] + 1 != nums[i + 1]) {
            Result* tmp = (Result*)malloc(sizeof(Result));
            tmp->smr = formatResult(nums[start], nums[i]);
            tmp->next = NULL;
            head = append(head, tmp);
            start = i + 1;
            (*returnSize)++;
        }
    }
    Result* tmp = (Result*)malloc(sizeof(Result));
    tmp->smr = formatResult(nums[start], nums[i]);
    tmp->next = NULL;
    head = append(head, tmp);
    (*returnSize) += 1;

    // 将链表转为数组
    // 在调用者中释放
    char** result = (char**)malloc(sizeof(char*) * (*returnSize));
    tmp = head;
    head = head->next;
    free(tmp);
    for (i = 0; head->next != NULL; i++) {
        result[i] = head->smr;

        tmp = head;
        head = head->next;
        free(tmp);
    }
    result[i] = head->smr;
    return result;
}
```
[返回首页](../README.md)