<!--
 * @Author: Hiseh
 * @Date: 2019-12-16 14:49:11
 * @LastEditors  : Hiseh
 * @LastEditTime : 2019-12-30 16:24:00
 * @Description: 
 -->
# 强整数
[返回首页](../README.md)

强整数定义不难理解，解题思路就是找到所有指数可能组合，显然可以用笛卡尔积实现。
## Python
用python自带的库函数，3行代码搞定
```python
import math
import itertools


class Solution:
    def powerfulIntegers(self, x: int, y: int, bound: int) -> list[int]:
        # x指数范围，减少计算量。注意，如果底数为1，对数函数会溢出，此时需要写死为1。
        x_log = 1 if x == 1 else int(math.log(bound, x)) + 1
        # 同理，y指数范围
        y_log = 1 if y == 1 else int(math.log(bound, y)) + 1

        # 1、生成0～x_log和0～y_log两个集合的笛卡尔积
        # 2、取出所有符合强整数定义的指数组合
        # 3、把结果存入集合
        # 4、用hash保证结果唯一
        return set(
            map(
                lambda e: x**e[0] + y**e[1],
                filter(lambda e: x**e[0] + y**e[1] <= bound,
                       itertools.product(range(x_log), range(y_log)))))
```
---

## C
用Python思路，需要自己实现笛卡尔积。另外c没有现成的函数可以计算以任意自然数为底的对数（标准库只有以*e/2/10*为底的对数），还好我们上过初中，知道*log<sub>n</sub><sup>a</sup> ÷ log<sub>n</sub><sup>b</sup> = log<sub>b</sub><sup>a</sup>*。
```c
#include <math.h>
#include <stdlib.h>
#include "uthash.h"

// 用uthash构建一个hash表
typedef struct {
    int powerful_value;
    UT_hash_handle hh;
} Powerful;
Powerful* powerful = NULL;

void cart_prod(int arr1[], int arr2[], int arr1_size, int arr2_size, int** result) {
    //  两个整数数组的笛卡尔积
    // 通常我们用遍历树的形式实现笛卡尔积，但这个题有点特殊，每个组合就固定的2个数，所以我用取巧的办法，循环嵌套实现
    int r_i = 0;
    for (int i = 0; i < arr1_size; i++) {
        for (int j = 0; j < arr2_size; j++) {
            // 因为result是个二维数组，所以每次循环写入两个元素
            *((int*)result + (r_i++)) = arr1[i];
            *((int*)result + (r_i++)) = arr2[j];
        }
    }
}

int* powerfulIntegers(int x, int y, int bound, int* returnSize) {
    // 同Python，分别计算x和y最大的指数
    int x_log = x == 1 ? 1 : (int)(log(bound) / log(x)) + 1;
    int y_log = y == 1 ? 1 : (int)(log(bound) / log(y)) + 1;

    // 获得笛卡尔积
    int x_arr[x_log];
    for (int i = 0; i < x_log; i++) {
        x_arr[i] = i;
    }
    int y_arr[y_log];
    for (int i = 0; i < y_log; i++) {
        y_arr[i] = i;
    }

    int cart_size = x_log * y_log;
    int cart_arr[cart_size][2];
    cart_prod(x_arr, y_arr, x_log, y_log, (int**)cart_arr);

    // 把结果存入hash表
    Powerful* tmp;
    *returnSize = 0;
    for (int i = 0; i < cart_size; i++) {
        if (powf(x, cart_arr[i][0]) + powf(y, cart_arr[i][1]) <= bound) {
            int powerful_value =
                powf(x, cart_arr[i][0]) + powf(y, cart_arr[i][1]);

            HASH_FIND_INT(powerful, &powerful_value, tmp);
            if (tmp == NULL) {
                tmp = (Powerful*)malloc(sizeof(Powerful));
                tmp->powerful_value = powerful_value;
                HASH_ADD_INT(powerful, powerful_value, tmp);
                (*returnSize)++;
            }
        }
    }

    // 存成数组
    int* result = (int*)malloc(sizeof(int) * (*returnSize));
    Powerful* curr;
    int i = 0;
    HASH_ITER(hh, powerful, curr, tmp) {
        result[i++] = curr->powerful_value;
        HASH_DEL(powerful, curr);
    }
    return result;
}

```
[返回首页](../README.md)