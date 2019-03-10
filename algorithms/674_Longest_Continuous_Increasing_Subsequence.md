#  最长的连续递增序列长度
[返回首页](../README.md)

关键词是**连续**，思路就是用循环遍历，遇到第一个不递增的就说明当前递增序列结束。
## Python
```python
class Solution:
    def findLengthOfLCIS(self, nums: list[int]) -> int:
        if len(nums) == 0:
            return 0
        else:
            tmp, result = 1, 1

        for i, v in enumerate(nums):
            try:
                if v < nums[i + 1]:
                    tmp += 1
                else:
                    result = max(result, tmp)
                    tmp = 1
            # 防止下标越界
            except IndexError:
                break
        return max(result, tmp)
```
---

## C
思路同Python
```c
int findLengthOfLCIS(int* nums, int numsSize) {
    if (numsSize == 0)
        return 0;

    int tmp = 1, result = 1;

    for (int i = 1; i < numsSize; i++) {
        if (nums[i - 1] < nums[i]) {
            tmp++;
        } else {
            result = tmp > result ? tmp : result;
            tmp = 1;
        }
    }
    return tmp > result ? tmp : result;
}
```
[返回首页](../README.md)