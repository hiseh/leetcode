# 二分法查找
[返回首页](../README.md)

## Python
Python自带的查找算法是[BM算法](https://baike.baidu.com/item/BM算法)，写Python就不自造轮子了，用默认的算法挺好的
```python
class Solution:
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        try:
            result = nums.index(target)
        except ValueError:
            result = -1
        finally:
            return result
```
---

## C
c老老实实用二分法实现。
```c
#include <string.h>

int search(int* nums, int numsSize, int target) {
    // 定义搜索边界
    int lIndex = 0;
    int rIndex = numsSize - 1;

    // 因为nums已经排好序，根据mIndex位置元素与target比较大小即可确定是向左还是向右查找
    while (lIndex <= rIndex) {
        int mIndex = (lIndex + rIndex) / 2;
        if (target == nums[mIndex]) {
            return mIndex;
        } else if (target < nums[mIndex]) {
            rIndex = mIndex - 1;
        } else {
            lIndex = mIndex + 1;
        }
    }
    return -1;
}
```
[返回首页](../README.md)