# 删除排序数组中的重复项
[返回首页](../README.md)

这道题绕弯的一点就是有空间复杂度要求，要求[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)操作，空间复杂度**O(1)**，也就是只能在原数组上做操作。
## Python
Python的slice notation就是指针操作改变截取内存地址，除非把结果赋给其它变量，否则内存地址不会变，也就是典型的in-place算法。可以用`id()`来验证操作前后的变量地址。
```python
class Solution(object):
    def removeDuplicates(self, nums: List[int]) -> int:
        # 用hash表特性去重
        nums[:] = sorted(set(nums))
        return len(nums)
```
---

## C
C没有现成的`in-place`的Hash库，换个思路实现。既然题目说了数组是排好序的，那么如果n == n+1，说明这就是重复项，这样遍历一遍数组就可以筛选出重复项。遍历过程中，将非重复项放到第一个重复项的位置，也不会占用额外空间。
```c
int removeDuplicates(int* nums, int numsSize) {
    if (numsSize == 0)
        return 0;
    
    // 慢指针，也就是去掉重复项后的指针
    int i = 0;
    for (int j = 1; j < numsSize; j++) {
        // 如果是非重复项，将非重复项内容放置到第一个重复项位置
        // 否则增加j，继续找下一个非重复项
        if (nums[j] != nums[i]) {
            i++;
            nums[i] = nums[j];
        }
    }
    return i + 1;
}
```
[返回首页](../README.md)