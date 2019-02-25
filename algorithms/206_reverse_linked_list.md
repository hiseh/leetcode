# 反转单链表
[返回首页](../README.md)

## Python
通常可以用递归和循环两种方式反转，我这里用循环方式解决。看图说话，一目了然![反转单链表](https://i2.wp.com/algorithms.tutorialhorizon.com/files/2014/08/Linked-List-Reversal.png)
```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if head == None:
            return head
        
        new_list = None
        next_node = None
        while head:
            next_node = head.next
            head.next = new_list
            new_list = head
            head = next_node

        return new_list
```
---

## C
```c
#include <stdlib.h>

struct ListNode {
    int val;
    struct ListNode* next;
};

struct ListNode* reverseList(struct ListNode* head) {
    struct ListNode *new_list = 0, *next_node;
    while (head) {
        next_node = head->next;
        head->next = new_list;
        new_list = head;
        head = next_node;
    }
    return new_list;
}
```
[返回首页](../README.md)