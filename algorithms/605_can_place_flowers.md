# 种花问题
[返回首页](../README.md)

假设你有一个很长的花坛，一部分地块种植了花，另一部分却没有。可是，花卉不能种植在相邻的地块上，它们会争夺水源，两者都会死去。

给定一个花坛（表示为一个数组包含0和1，其中0表示没种植花，1表示种植了花），和一个数 n 。能否在不打破种植规则的情况下种入 n 朵花？能则返回True，不能则返回False。

## Python
猛一看有点像相邻房间问题，实际想一下，这道题比相邻房间要简单，只要值为1的元素不相邻即可满足题目要求，最后算一下这些元素数量是不是大于n即可。
```python
class Solution(object):
    def canPlaceFlowers(self, flowerbed, n):
        """
        :type flowerbed: List[int]
        :type n: int
        :rtype: bool
        """
        # 理论上总数量不能大于花坛长度的1/2，实际上还要去掉已种的棵数。
        ideal_max = (len(flowerbed) + 1) // 2
        if n > ideal_max or n > (ideal_max - flowerbed.count(1)):
            return False

        if n == 0:
            return True

        flower = 0
        max_i = len(flowerbed)
        for i in range(max_i):
            # 值为1的元素不相邻，说明可以种一颗，+1
            if flowerbed[i] == 0 and (i == 0 or flowerbed[i - 1] == 0) and (
                    i == max_i - 1 or flowerbed[i + 1] == 0):
                flowerbed[i] = 1
                flower += 1

        return flower >= n
```
---

## C
同Python思路一样，用C重写一次即可。
```c
#include <stdbool.h>

bool canPlaceFlowers(int* flowerbed, int flowerbedSize, int n) {
    int ideal_max = (flowerbedSize + 1) / 2;
    // C中想算出值为1的元素数量，还得遍历一遍数组，少遍历一次数组，下面多算点异常情况，总的时间成本差不多，还能少写点代码。
    if (n > ideal_max)
        return false;

    if (n == 0)
        return true;

    int flower = 0;
    for (int i = 0; i < flowerbedSize; i++) {
        if (flowerbed[i] == 0 && (i == 0 || flowerbed[i - 1] == 0) &&
            (i == flowerbedSize - 1 || flowerbed[i + 1] == 0)) {
            flowerbed[i] = 1;
            flower += 1;
        }
    }

    return flower >= n;
}
```
[返回首页](../README.md)