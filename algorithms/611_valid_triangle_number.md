<!--
 * @Author: Hiseh
 * @Date: 2019-12-16 14:49:11
 * @LastEditors  : Hiseh
 * @LastEditTime : 2019-12-31 14:04:38
 * @Description: 
 -->
# 有效三角形的个数
[返回首页](../README.md)

最简单的办法是暴力破解，直接*C<sub>3</sub><sup>n</sup>*，然后根据三角形三边关系检查。这么做时间和空间复杂度都相当高，为*θ(n･2<sup>n</sup>)*，显然不适用leetcode场合。

## Python
```python
# 不考虑时间复杂度，一行代码搞定
len(list(filter(lambda e: sum(e[0:2]) > e[2], map(sorted, itertools.combinations(nums, 3)))))
```
上面代码在leetcode运行时会超时，因此我们必须换个思路。如果有序数组中，最大的元素 < 其它两个元素之和，那么肯定这三个元素可以组成一个三角形。通过循环嵌套遍历一次，时间复杂度为*θ(n<sup>2</sup>)*。
```python
class Solution:
    def triangleNumber(self, nums: list) -> int:
        nums, count, nums_size = sorted(nums, reverse=1), 0, len(nums)
        for max_i in range(nums_size):
            sec_max_i, min_i = max_i + 1, nums_size - 1
            while sec_max_i < min_i:
                if nums[sec_max_i] + nums[min_i] > nums[max_i]:
                    # 说明两个坐标之间的值都符合
                    count += min_i - sec_max_i
                    sec_max_i += 1
                else:
                    min_i -= 1
        return count
```
---

## C
照搬Python思路。
```c
#include <stdlib.h>
int compare(const void* p1, const void* p2) { return *(int*)p2 - *(int*)p1; }

int triangleNumber(int* nums, int numsSize) {
    int count = 0;
    qsort(nums, numsSize, sizeof(int), compare);
    for (int max_i = 0; max_i < numsSize; max_i++) {
        int sec_max_i = max_i + 1;
        int min_i = numsSize - 1;
        while (sec_max_i < min_i) {
            if (nums[sec_max_i] + nums[min_i] > nums[max_i]) {
                count += min_i - sec_max_i;
                sec_max_i++;
            } else {
                min_i--;
            }
        }
    }
    return count;
}
```
[返回首页](../README.md)