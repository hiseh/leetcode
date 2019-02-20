# 移除链表元素
[返回首页](../README.md)

给定一个只包括 `(`，`)`，`{`，`}`，`[`，`]`的字符串，判断括号是否相匹配。
## Python
考虑到括号嵌套时也要验证顺序是否匹配，所以这里正则就无能为力了。也就是说要考虑这种情况：`{...[..(xxx)xx]x}`，从这个例子我们可以发现匹配的字符串有如下规律：

0. 括号总是成套的，并且从字符串左往右和从右往左，成套的括号出现位置是相同的（去掉非括号元素的影响）
0. 当成套的括号都取走后，字符串应该不包含任何括号。

从这两个规律可以选定用**栈**来实现了，后入先出，弹完必须是空的。
```python
class Solution:
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        if not s or 0 == len(s):
            return True

        # 下面会用这个dict来校验括号是否成套
        parentheses_dict = {'(': ')', '{': '}', '[': ']'}

        # 用数组来实现栈
        stack = []
        for c in s:

            # 遇到做括号，把对应的右括号压进栈里
            if c in parentheses_dict:
                stack.append(parentheses_dict[c])

            # 遇到右括号，从栈里弹出一个元素，检查弹出的元素是否等于右括号。如果不想等，则违背了上面第0条规律。
            elif c in parentheses_dict.values():
                try:
                    if c != stack.pop():
                        return False
                except IndexError:
                    return False
        
        # 检查遍历完字符串后，栈是否为空，如果不为空则违背了上面第1条规律。
        return stack == []
```
---

## C
思路与Python一样，这里用链表实现栈。如果考虑用空间换时间，可以用数组来实现栈，数组长度就设定为`(strlen(s) + 1) / 2`。
```c
#include <stdbool.h>
#include <stdlib.h>
#include <string.h>

typedef struct Stack {
    char val;
    struct Stack* next;
} Stack;

bool isValid(char* s) {
    if (strlen(s) == 0)
        return true;

    Stack* root = NULL;
    while (*s != '\0') {
        if (*s == '(' || *s == '{' || *s == '[') {
            Stack* e = (Stack*)malloc(sizeof(Stack));
            switch (*s) {
                case '(':
                    e->val = ')';
                    break;
                case '{':
                    e->val = '}';
                    break;
                case '[':
                    e->val = ']';
                    break;
                default:
                    return false;
                    break;
            }
            e->next = root;
            root = e;
        } else if (*s == ')' || *s == '}' || *s == ']') {
            if (root != NULL) {
                Stack* tmp = root;
                char c = root->val;
                root = root->next;
                free(tmp);
                if (c != *s) {
                    while (root != NULL) {
                        Stack* tmp = root;
                        root = root->next;
                        free(tmp);
                    }
                    return false;
                }
            } else {
                return false;
            }
        }
        s++;
    }
    return root == NULL;
}
```
[返回首页](../README.md)