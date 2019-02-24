# 搜索插入位置
[返回首页](../README.md)

给定一个无重复的排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
## Python
```python
class Solution:
    def searchInsert(self, nums: 'List[int]', target: 'int') -> 'int':
        result = 0
        try:
            result = nums.index(target)
        except ValueError:
            # 如果目标值<数组最大值，去查找目标值应该插入的下标，否则插入到数组最后一个值后面
            if target < nums[-1]:
                for i, e in enumerate(nums):
                    if e > target:
                        result = i
                        break
            else:
                result = len(nums)
        return result
```
---

## C
优化一下Python思路，遍历数组时，发现第一个比目标元素大的值，就是要插入的坐标。
```c
int searchInsert(int* nums, int numsSize, int target) {
    int result = 0;
    if (nums[numsSize - 1] < target) {
        result = numsSize;
    } else {
        for (; result < numsSize; result++) {
            if (nums[result] >= target) {
                break;
            }
        }
    }
    return result;
}
```
[返回首页](../README.md)