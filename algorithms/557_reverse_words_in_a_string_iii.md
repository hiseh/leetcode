# 反转字符串中的单词 III
给定一个字符串，反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。
> 输入: "Let's take LeetCode contest"<br/>
> 输出: "s'teL ekat edoCteeL tsetnoc" 


[返回首页](../README.md)

## Python
还记得Python伟大的slice notation吧，一行搞定
```python
class Solution:
    def reverseWords(self, s: str) -> str:
        # 1、用空格把字符串劈成数组
        # 2、反转每个元素
        # 3、把数组用空格拼成字符串
        return ' '.join([v[::-1] for v in s.split(' ')])
```
---

## C
借鉴Python思路，用C写一遍
```c
char* reverseWords(char* s) {
    char *begin, *end, *tmp_p;

    // 遍历字符串
    for (begin = end = s;; ++end) {
        // 用空格或结尾判断单词结束
        if (*end == ' ' || *end == '\0') {

            // 颠倒单词字符串
            for (tmp_p = end - 1; begin < tmp_p; begin++, tmp_p--) {
                // 交换单词开头和结尾字符
                // 也可以用一个临时变量做交换
                *begin = *begin^*tmp_p;
                *tmp_p = *begin^*tmp_p;
                *begin = *begin^*tmp_p;

            }

            // 下一个单词
            begin = end + 1;
        }
        if (*end == '\0')
            break;
    }
    return s;
}
```
[返回首页](../README.md)