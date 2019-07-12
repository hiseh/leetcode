# 子数组最大平均数 I
[返回首页](../README.md)

## Python
```python
class Solution:
    def findMaxAverage(self, nums: list, k: int) -> float:
        tmp_sum = max_sum = sum(nums[:k])

        for i in range(1,len(nums) - k + 1):
            tmp_sum = tmp_sum - nums[i - 1] + nums[i + k - 1]
            if tmp_sum > max_sum:
                max_sum = tmp_sum
                
        return max_sum / k
```
---

## C
循环次数不多，用`long`还是`double`，效率没什么差别。
```c
double findMaxAverage(int* nums, int numsSize, int k) {
    long tmp_sum = 0, max_sum = 0;
    for (int i = 0; i < k; i++) {
        max_sum += nums[i];
    }

    tmp_sum = max_sum;

    for (int i = 1; i <= numsSize - k; i++) {
        tmp_sum -= nums[i - 1] - nums[i + k - 1];
        if (tmp_sum > max_sum) {
            max_sum = tmp_sum;
        }
    }

    return (double)max_sum / k;
}
```
[返回首页](../README.md)