# 查找常用字符
[返回首页](../README.md)

## Python
使用Python库函数，一行搞定
```python
import collections
import functools

class Solution:
    def commonChars(self, A: List[str]) -> List[str]:
        '''
        参考官网文档Counter类的例子：两个Counter对象按位与，可以返回其交集。通过reduce函数可以依次将交集与所有Counter对象取交集，最后的结果就是所有字符串中都包含的公共字符。

        一个更易理解的方式：
        c = collections.Counter(A[0])
        for word in A:
            c &= collections.Counter(word)
        return list(c.elements()
        '''
       return list(functools.reduce(collections.Counter.__and__, map(collections.Counter, A)).elements())
```
---

## C
```c
```
[返回首页](../README.md)