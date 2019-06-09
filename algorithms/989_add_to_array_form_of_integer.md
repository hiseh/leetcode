# 数组形式的整数加法
[返回首页](../README.md)

思路很简单就是

0. 把A转成数字M
0. M+K结果存入R
0. 把R转成数组
## Python
因为Python使用数组保存数字
```c
// [longintrepr.h]
struct _longobject {
    PyObject_VAR_HEAD
    int *ob_digit;
};
```
可支持的最大数字仅受可用内存限制，不需要考虑溢出问题。

因此，一行代码搞定。
```python
class Solution:
    def addToArrayForm(self, A: list[int], K: int) -> list[int]:
        return [int(e) for e in str(int(''.join(map(str, A))) + K)]
```
---

## C
C当然不会有像python那么高大上的数字处理机制了，因此必须考虑数字长度问题。题目里说`A.length <= 10000`，那`long long`肯定也不够大，学Python，用数组存数字。
```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */

#include <stdlib.h>

int* addToArrayForm(int* A, int ASize, int K, int* returnSize) {
    // K的长度
    int KSize = 0;
    for (int i = K; i > 0; i /= 10)
        KSize++;

    // 返回数组的长度，因为是加法，所以最多进一位
    int rSize = (KSize > ASize) ? (KSize + 1) : (ASize + 1);

    // 把结果存到临时数组
    // !注意，这个临时数组顺序是反的
    int* tmp_arr = (int*)malloc(rSize * sizeof(int));
    int idx = 0;
    for (int i = ASize - 1; (i >= 0) || (K > 0); i--) {
        // 因为K <= 10000，所以直接K+A[i]即可
        K += (i >= 0) ? A[i] : 0;

        // 没办法把完整数字存到内存里，所以这里每次存最低位，存完抛弃最低位。从左往右存代码简单些
        // 如果有时间，可以借鉴Python处理方式，把数字分为三块，分别放在高中低三个数组里，运算时可抑制处理低位数组
        tmp_arr[idx] = K % 10;
        idx++;
        K /= 10;
    }

    // 把临时数组存到result_arr
    *returnSize = tmp_arr[rSize - 1] == 0 ? rSize - 1 : rSize;
    int* result_arr = (int*)malloc((*returnSize) * sizeof(int));
    for (int j = 0, i = returnSize - 1; i >= 0; j++, i--) {
        result_arr[j] = tmp_arr[i];
    }
    free(tmp_arr);

    return result_arr;
}
```
[返回首页](../README.md)