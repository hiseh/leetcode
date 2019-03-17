# 字符的最短距离
[返回首页](../README.md)

给定一个字符串 S 和一个字符 C。返回一个代表字符串 S 中每个字符到字符串 S 中的字符 C 的最短距离的数组。

> **输入:** S = "loveleetcode", C = 'e'<br/>
> **输出:** [3, 2, 1, 0, 1, 0, 0, 1, 2, 2, 1, 0]

两种思路：

0. 遍历字符串，分别向左和向右查找给定字符的下标，如果两侧都能找到，则取最小值，否则取有结果的那个值。这种方法虽然直观好理解，但每次都要查找2个字符的下标，效率较低。
0. 假设第n个字符向左查找C的下标是i1，向右是i2。如果i2>0那么n+1个字符对应的两个下标肯定是i1+1和i2-1；如果i2=0，那么两个下标肯定是1和一个新的值。用这个规律，遍历数组时只需要当i2=0时，才需要找下一个右侧下标。
## Python
两个方法分别实现。
```python
class Solution:
    def shortestToChar_1(self, S: str, C: str) -> List[int]:
        """
        第一种思路
        """
        result = []
        max_l = len(S)
        for i in range(max_l):
            # 分别从左和从右查找C的下标
            left_c_i = S.rfind(C, 0, i)
            right_c_i = S.find(C, i)
            if -1 == left_c_i:
                result.append(right_c_i - i)
            elif -1 == right_c_i:
                result.append(i - left_c_i)
            else:
                result.append(min(i - left_c_i, right_c_i - i))

        return result

    def shortestToChar(self, S: str, C: str) -> List[int]:
        """
        第二种思路
        """
        result = []
        max_l = len(S)
        left_c_i = 0 if S[0] == C else -1
        right_c_i = S.find(C, 0)

        result.append(0 if left_c_i == 0 else right_c_i)
        for i in range(1, max_l):
            if left_c_i == -1:
                if right_c_i > 0:
                    # 左侧没有
                    left_c_i = -1
                    right_c_i -= 1
                elif right_c_i == 0:
                    # 挪出第一个右侧
                    left_c_i = 1
                    right_c_i = S.find(C, i)
                    right_c_i = right_c_i - i if right_c_i > -1 else right_c_i
            else:
                if right_c_i > 0:
                    left_c_i += 1
                    right_c_i -= 1
                elif right_c_i == 0:
                    # 挪出第一个右侧
                    left_c_i = 1
                    right_c_i = S.find(C, i)
                    right_c_i = right_c_i - i if right_c_i > -1 else right_c_i
                else:
                    left_c_i += 1

            if left_c_i == -1:
                result.append(right_c_i)
            elif -1 == right_c_i:
                result.append(left_c_i)
            else:
                result.append(min(left_c_i, right_c_i))

        return result
```
---

## C
用第二种思路实现
```c
#include <stdlib.h>
#include <string.h>

int find(char* string, char sub, size_t start) {
    // 返回start之后第一个sub的位置
    size_t str_l = strlen(string);

    int result = 0;
    if (str_l - 1 < start) {
        // 超过结尾
        result = -1;
    } else if (str_l - 1 == start) {
        // 最后一位
        result = string[start] == sub ? start : -1;
    } else {
        // 获取substring
        char* sub_str = (char*)malloc((str_l - start + 1) * sizeof(char));
        strcpy(sub_str, string + start);
        char* right_c = strchr(sub_str, sub);
        result = right_c ? right_c - sub_str + start : -1;
        free(sub_str);
    }
    return result;
}

int* shortestToChar(char* S, char C, int* returnSize) {
    size_t max_l = strlen(S);
    *returnSize = max_l;
    int* result = (int*)malloc(max_l * sizeof(int));

    int left_c_i = S[0] == C ? 0 : -1;
    int right_c_i = find(S, C, 0);
    char* righ_c = strchr(S, C);
    if (righ_c) {
        right_c_i = righ_c - S;
    }

    result[0] = left_c_i == 0 ? 0 : right_c_i;
    for (size_t i = 1; i < max_l; i++) {
        if (left_c_i == -1) {
            if (right_c_i > 0) {
                // 左侧没有
                left_c_i = -1;
                right_c_i--;
            } else if (right_c_i == 0) {
                // 挪出第一个右侧
                left_c_i = 1;

                right_c_i = find(S, C, i);
                right_c_i = right_c_i > -1 ? right_c_i - i : right_c_i;
            }
        } else {
            if (right_c_i > 0) {
                left_c_i++;
                right_c_i--;
            } else if (right_c_i == 0) {
                // 挪出第一个右侧
                left_c_i = 1;
                right_c_i = find(S, C, i);
                right_c_i = right_c_i > -1 ? right_c_i - i : right_c_i;
            } else {
                left_c_i++;
            }
        }

        if (left_c_i == -1) {
            result[i] = right_c_i;
        } else if (right_c_i == -1) {
            result[i] = left_c_i;
        } else {
            result[i] = left_c_i > right_c_i ? right_c_i : left_c_i;
        }
    }

    return result;
}
```
[返回首页](../README.md)