<!--
 * @Author: hiseh
 * @Date: 2019-07-30 16:15:54
 * @LastEditors: Hiseh
 * @LastEditTime: 2019-08-19 21:30:55
 * @Description: 寻找最近的回文数
 -->
# 寻找最近的回文数
[返回首页](../README.md)

这道题目不难理解，复杂的地方就是要考虑的因素很多，很繁琐。为了简化代码，可以把一些特殊情况写死，如代码所示。

## Python
```python
class Solution:
    def nearestPalindromic(self, n: str) -> str:
        if n == '0':
            return '1'

        if len(n) == 1:
            return str(int(n) - 1)

        if int(n) <= 11:
            return '9'

        if int(n) <= 16:
            return '11'

        # 回文数三种情况：1、直接用前半段做回文数。2、前半段-1。3、前半段+1。
        if len(n) % 2 == 0:
            left_n = n[0:len(n) // 2]
            left_i = str(int(left_n) + 1)
            left_r = str(int(left_n) - 1)

            # 如果-1后出现借位情况，较小的回文数就是n-1个9
            palindrome_min = '{}{}'.format(left_r, left_r[::-1]) if len(
                left_r) == len(left_n) else '9' * (len(n) - 1)
            palindrome_mid = '{}{}'.format(left_n, left_n[::-1])

            # 如果出现进位情况，较大的回文数就是1+(n-1)个0+1
            palindrome_max = '{}{}'.format(left_i, left_i[::-1]) if len(
                left_i) == len(left_n) else '1{}1'.format('0' * (len(n) - 1))
        else:
            left_n = n[0:len(n) // 2 + 1]
            left_i = str(int(left_n) + 1)
            left_r = str(int(left_n) - 1)

            palindrome_min = '{}{}'.format(left_r, left_r[len(left_r) - 2::-1]) if len(
                left_r) == len(left_n) else '9' * (len(n) - 1)
            palindrome_mid = '{}{}'.format(left_n, left_n[len(left_n) - 2::-1])
            palindrome_max = '{}{}'.format(left_i, left_i[len(left_i) - 2::-1]) if len(
                left_i) == len(left_n) else '1{}1'.format('0' * (len(n) - 1))

        # 根据题目要求，依次判断哪个回文数是“最接近”的回文数
        max_a = abs(int(palindrome_max) - int(n))
        min_a = abs(int(palindrome_min) - int(n))
        mid_a = abs(int(palindrome_mid) - int(n))
        if mid_a != 0 and mid_a <= max_a and mid_a < min_a:
            return str(palindrome_mid)
        if min_a <= max_a and (min_a <= mid_a or mid_a == 0):
            return str(palindrome_min)
        if max_a < min_a and (max_a < mid_a or mid_a == 0):
            return str(palindrome_max)
```
---

## C
思路同Python，不写了。
```c
```
[返回首页](../README.md)