<!--
 * @Author: Hiseh
 * @Date: 2019-10-23 17:24:14
 * @LastEditors: Hiseh
 * @LastEditTime: 2019-10-23 20:45:38
 * @Description: 
 -->
#  移动石子直到连续
[返回首页](../README.md)

注意题目里并没有说 *a<sub>x</sub> < b<sub>x</sub> < c<sub>x</sub>* ，所以必须先排序。求最大次数很简单，无论 *y* 在什么位置，只要计算出 *z - x - 2* 即可，也就是 *z - y - 1 + y - x - 1*。算最小次数要分情况算：

1. 三颗石子的位置是连续时，不需要移动，所以次数是0。
1. 有两颗石子的位置是连续的，或间隔**1**时，只需要移动一次。也就是说第三颗石子可以一次移动到中间石子的旁边或两颗之间，一步到位。
1. 除此之外需要移动两次。

## Python
用上面思路，很容易实现。
```python
class Solution:
    def numMovesStones(self, a: int, b: int, c: int) -> list:
        stones = sorted([a, b, c])

        if stones[2] - stones[0] == 2:
            min_mov = 0
        elif stones[2] - stones[1] < 3 or stones[1] - stones[0] < 3:
            min_mov = 1
        else:
            min_mov = 2

        return [min_mov, stones[2] - stones[0] - 2
```
---

## C
用C重写Python代码即可。
```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
#include <stdlib.h>

int compare(const void* p1, const void* p2) { return *(int*)p1 - *(int*)p2; }

int* numMovesStones(int a, int b, int c, int* returnSize) {
    int stones[] = {a, b, c};
    qsort(stones, 3, sizeof(int), compare);
    *returnSize = 2;
    int* result = (int*)malloc(sizeof(int*) * (*returnSize));
    int min_mov;
    if (stones[2] - stones[0] == 2) {
        min_mov = 0;
    } else if (stones[2] - stones[1] < 3 || stones[1] - stones[0] < 3) {
        min_mov = 1;
    } else {
        min_mov = 2;
    }
    result[0] = min_mov;
    result[1] = stones[2] - stones[0] - 2;
    return result;
}
```
[返回首页](../README.md)