# 给定数字能组成的最大时间
[返回首页](../README.md)

首先题目中说了，输入条件是4个数字组成的数组，那么排列一遍也不过有4!种可能，空间和时间占用上限都不大，可以接受。
## Python
利用Python标准库里的itertools模块，轻易搞定。
```python
# -1表示不能组成有效时间
result = -1

# 排列输入数组，获得数组中4个数字作为小时和分钟数
for h1, h2, m1, m2 in itertools.permutations(A):
    hours = 10 * h1 + h2
    mins = 10 * m1 + m2

    # 拼出总时间，通过总时间检查是否是最大时间
    time = 60 * hours + mins
    if 0 < hours < 24 and 0 < mins < 60 and time > result:
        result = time

# 格式化输出
return '{:02}:{:02}'.format(*divmod(result, 60)) if result > -1 else ''
```
---

## C
c标准库里没有排列函数，自己实现一个。考虑到最多就是24种组合，用递归方式实现最简单，都不用优化成尾递归。除了排列函数，其它部分逻辑可照搬Python的。
排列算法也不复杂，借鉴打枪法（闺女奥数课的术语，很形象）思路，让元素两两交换位置。用递归方式实现的思路如下，这种方式不会过滤掉重复的排列：
![递归](https://www.geeksforgeeks.org/wp-content/uploads/NewPermutation.gif)
```c
#include <stdio.h>
#include <stdlib.h>

// 交换两个元素
void swap(int* a, int x, int y) {
    int temp;
    temp = a[y];
    a[y] = a[x];
    a[x] = temp;
}

void permute(int* a, int l, int r, int** res, int* row, int col) {
    if (l == r) {
        // 完成一次排列，将结果保存到res二维数组中
        for (int i = 0; i <= r; i++) {
            *((int*)res + (*row) * col + i) = a[i];
        }
        
        // 完成一次加一行
        *row = *row + 1;
        return;
    } else {
        for (int i = l; i <= r; i++) {
            swap(a, i, l);
            permute(a, l + 1, r, res, row, col);
            swap(a, i, l);
        }
    }
}

char* largestTimeFromDigits(int* A, int ASize) {
    int max = 1;
    // 算一下排列结果数量
    for (int i = ASize; i > 0; i--) {
        max *= i;
    }

    // 存储排列结果
    int row = 0;
    int resultArr[max][ASize];
    permute(A, 0, ASize - 1, (int**)resultArr, &row, ASize);

    // 下面逻辑同Python
    int result = -1;
    for (int i = 0; i < max; i++) {
        int h1 = resultArr[i][0];
        int h2 = resultArr[i][1];
        int m1 = resultArr[i][2];
        int m2 = resultArr[i][3];

        int hours = 10 * h1 + h2;
        int mins = 10 * m1 + m2;
        int times = 60 * hours + mins;
        if ((hours < 24 && hours > 0) && (mins > 0 && mins < 60) && (times > result)) {
            result = times;
        }
    }

    if (result > -1) {
        // 正常返回是5个字符组成，申请内存空间，调用者里记得要释放
        char* resultStr = (char*)malloc(sizeof(char) * 5);

        // 格式化输出
        sprintf(resultStr, "%02d:%02d", result / 60, result % 60);
        return resultStr;
    } else {
        return "";
    }
}
```
[返回首页](../README.md)