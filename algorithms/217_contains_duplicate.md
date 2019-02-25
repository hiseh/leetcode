# 判断整数数组是否存在重复元素
[返回首页](../README.md)

## Python
一说到重复元素，自然想到hash表，将数组转成hash表，判断一下长度是否相等就可以了。
```python
class Solution:
    def containsDuplicate(self, nums: 'List[int]') -> 'bool':
        return len(nums) != len(set(nums))
```
---

## C
C可以用Hash，也可以用双层循环。
```c
#include <stdbool.h>
#include <stdlib.h>


// 假设数组最大长度为4096
static int table[4096];

bool containsDuplicate(int* nums, int numsSize) {
    int m = 16, n = numsSize;
    while (m < n) {
        m <<= 1;
    }
    m <<= 1;
    
    // 初始化大于numsSize 2倍的内存空间，如果本行编译报错，或者运行崩溃，说明table长度过小，应该大于等于m * sizeof(int)
    memset(table, 0, m * sizeof(int));
    int *is = table, *ip = is, *ie = is + m;
    int zcount = 0;
    m--;
    int *ps = nums, *pe = ps + n, *p = ps;
    for (; p < pe; ++p) {

        // 检查数字是否重复
        int* ip = is + (*p & m);
        for (; *ip != 0;) {
            if (*ip == *p) {
                return true;
            }
            if (++ip == ie) {
                ip = is;
            }
        }
        if (*p == 0 && zcount++ >= 1) {
            return true;
        }
        *ip = *p;
    }
    return false;
}
```
[返回首页](../README.md)