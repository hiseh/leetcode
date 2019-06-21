# 最小栈
[返回首页](../README.md)
自己实现个栈，稍微麻烦点的是要求获取最小元素的时间复杂度是Θ(1)。
## Python
用现成的库。
> Python中有3个获取最小值的方法：<br>
> 1. 内置的min函数<br>
> 2. heapq.nsmallest<br>
> 3. 内置的sorted函数<br>
> 这里只用返回唯一最小值，所以用min最快，也最简便。
```python
import collections


class MinStack:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = collections.deque()

    def push(self, x: int) -> None:
        self.stack.append(x)

    def pop(self) -> None:
        try:
            self.stack.pop()
        except IndexError:
            pass

    def top(self) -> int:
        try:
            return self.stack[-1]
        except IndexError:
            return None

    def getMin(self) -> int:
        return min(self.stack)
```
---

## C
C没有min函数，没法像Python那么方便获取最小值。这里用了一个取巧的办法：每个元素保存**当前**的最小值。多个item的结构类似
|id|当前最小值|当前值|next|
|--------|--------|--------|-------|
|3|-3|-3|NULL|
|2|-2|0|3|
|1|-2|-2|2|
|0|INT32_MAX|INT32_MIN|1|
请看代码。
```c
#include <stdint.h>
#include <stdlib.h>

typedef struct MinStack {
    int min;  // 当前最小值
    int val;
    struct MinStack* next;
} MinStack;

MinStack* minStackCreate() {
    MinStack* tmp = (MinStack*)malloc(sizeof(MinStack));
    if (tmp) {
        tmp->min = INT32_MAX;  // 为了防止溢出，设个默认最小值用
        tmp->val = INT32_MIN;
        tmp->next = NULL;
    }
    return tmp;
}


void minStackPush(MinStack** obj, int x) {
    MinStack* tmp = minStackCreate();

    // 设置当前最小值
    tmp->min = x < (*obj)->min ? x : (*obj)->min;
    tmp->val = x;
    tmp->next = *obj;
    *obj = tmp;
}

void minStackPop(MinStack** obj) {
    MinStack* tmp = *obj;
    if (tmp->next != NULL) {
        *obj = tmp->next;
    }

    free(tmp);
}

int minStackTop(MinStack* obj) {
    if (obj->next != NULL) {
        return obj->val;
    }
    return INT32_MIN;
}

int minStackGetMin(MinStack* obj) {
    if (obj->next != NULL) {
        return obj->min;
    }
    return INT32_MIN;
}

void minStackFree(MinStack** obj) {
    while ((*obj)->next != NULL) {
        MinStack* tmp = *obj;
        *obj = tmp->next;
        free(tmp);
    }
}
```
[返回首页](../README.md)