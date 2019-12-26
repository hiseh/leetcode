<!--
 * @Author: Hiseh
 * @Date: 2019-12-16 14:49:11
 * @LastEditors  : Hiseh
 * @LastEditTime : 2019-12-26 17:47:11
 * @Description: 
 -->
# 长度最小的子数组 
[返回首页](../README.md)

按题目给的提示，最优解时间复杂度是*θ(nln<sup>n</sup>)*，因此优先考虑采用双指针形式：一个快指针，一个慢指针

## Python
```python
class Solution:
    def minSubArrayLen(self, s: int, nums: list) -> int:
        if sum(nums) < s:
            return 0

        # 慢指针
        r_p = 0
        nums_sum = 0
        result = nums_len = len(nums)

        # 快指针
        for l_p in range(nums_len):
            nums_sum += nums[l_p]

            # 从右向左遍历
            while nums_sum >= s:
                result = min(result, l_p - r_p + 1)
                nums_sum -= nums[r_p]
                r_p += 1

        return result
```
---

## C
采用python思路，只是C没有sum函数，为了减少一次循环相加，可以通过判断nums_sum变量有没有大于过s即可。
```c
static inline int min(int a, int b) {
    return a > b ? b : a;
}

int minSubArrayLen(int s, int* nums, int numsSize) {
    int is_zero = 0;

    int r_p = 0;
    int nums_sum = 0;
    int result = numsSize;

    for (int l_p = 0; l_p < numsSize; l_p++) {
        nums_sum += nums[l_p];
        while (nums_sum >= s) {
            is_zero = 1;

            result = min(result, l_p - r_p + 1);
            nums_sum -= nums[r_p++];
        }
    }
    return is_zero == 0 ? 0 : result;
}
```
[返回首页](../README.md)