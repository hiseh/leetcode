# 整数反转
## Python
利用Python自带的数组操作，5分钟搞定：
```python
# 先将数字转为字符串，然后反转字符串
m_str = str(x)[::-1]

if m_str[-1] == '-':
    # 如果是负数，把'-'挪到头里去
    m_str = '-' + m_str[0:-1]

# 转成整数
result = int(m_str)

# 检查是否溢出
return result if -2**31 <= result <= 2**31-1 else 0
```
---

## C
c语言缺少python那么有好的api，不能直接借鉴python的思路。我的思路是在循环里用求模方式从后往前取出每个数字，然后再拼成一个心的整数：
```c
#include <limits.h>
#include <stdlib.h>

int reverse(int x) {
    int32_t result = 0;
    int pop;

    // 检查输入值已经溢出
    if (x <= INT_MIN || x > INT_MAX)
        return 0;
    
    // 遍历当前整数
    while (x != 0) {
        // 获得最后一个数字
        pop = x % 10;

        // 剩下的整数
        x /= 10;

        // 检查预期结果是否溢出，如果溢出，直接返回0
        if (result > INT_MAX / 10 || (result == INT_MAX / 10 && pop > 7))
            return 0;
        if (result < INT_MIN / 10 || (result == INT_MIN / 10 && pop < -8))
            return 0;

        // 把原整数的最后一个整数放到返回结果的最后面
        result = result * 10 + pop;
    }
    return result;
}
```