# 用队列实现栈
[返回首页](../README.md)

## Python
用数组或deque都可以都可以实现栈，Python已经提供deque对象，很容易就能实现一个双向操作队列。
```python
from collections import deque

class MyStack:
    def __init__(self):
        """
        """
        self.stack = deque()

    def push(self, x: int) -> None:
        """
        Push element x onto stack.
        """
        self.stack.append(x)

    def pop(self) -> int:
        """
        Removes the element on top of the stack and returns that element.
        """
        try:
            return self.stack.pop()
        except IndexError:
            return None

    def top(self) -> int:
        """
        Get the top element.
        """
        try:
            return self.stack[0]
        except IndexError:
            return None

    def empty(self) -> bool:
        """
        Returns whether the stack is empty.
        """
        return False if self.stack else False
```
---

## C
C可以用数组或链表来实现队列，这道题给了`maxSize`参数，考虑用数组实现。
```c
#include <stdbool.h>
#include <stdio.h>

typedef struct {
    // 用数组模拟栈
    int top;
    int maxSize;
    int* array;
} MyStack;

bool myStackEmpty(MyStack* obj);

/** 初始化栈 **/
MyStack* myStackCreate(int maxSize) {
    MyStack* stack = (MyStack*)malloc(sizeof(MyStack));
    stack->top = -1;
    stack->maxSize = maxSize;
    stack->array = (int*)malloc(sizeof(int) * maxSize);
    return stack;
}

bool isFull(MyStack* obj) {
    return obj->top == obj->maxSize - 1 ? true : false;
}

/** Push element x onto stack. */
void myStackPush(MyStack* obj, int x) {
    if (!isFull(obj))
        obj->array[++obj->top] = x;
}

/** Removes the element on top of the stack and returns that element. */
int myStackPop(MyStack* obj) {
    if (myStackEmpty(obj))
        return NULL;

    return obj->array[obj->top--];
}

/** Get the top element. */
int myStackTop(MyStack* obj) {
    if (myStackEmpty(obj))
        return NULL;

    return obj->array[obj->top];
}

/** Returns whether the stack is empty. */
bool myStackEmpty(MyStack* obj) {
    return obj->top == -1 ? true : false;
}

void myStackFree(MyStack* obj) {
    free(obj->array);
    free(obj);
}
```
[返回首页](../README.md)