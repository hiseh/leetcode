# 移除链表元素
[返回首页](../README.md)

删除链表中等于给定值**val**的所有节点。
## Python
就是一个简单的链表操作，数据结构练习题。太简单了，我写了两种解决办法。
```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    def removeElements_recursive(self, head, val):
        '''
        递归方式
        '''
        if not head:
            return head

        head.next = self.removeElements_recursive(head.next, val)
        return head.next if val == head.val else head

    def removeElements(self, head, val):
        """
        传统循环
        :type head: ListNode
        :type val: int
        :rtype: ListNode
        """
        if not head:
            return head

        fake_head = ListNode(None)
        fake_head.next = head
        curr_node = head
        prev = fake_head
        while curr_node:
            if val == curr_node.val:
                prev.next = curr_node.next
            else:
                prev = prev.next
            curr_node = curr_node.next
        return fake_head.next
```
---

## C
实在没啥可说的，看代码吧。
```c
#include <stdlib.h>

struct ListNode {
    int val;
    struct ListNode* next;
};

struct ListNode* removeElements(struct ListNode* head, int val) {
    if (NULL == head)
        return head;

    struct ListNode* fake_head = NULL;
    fake_head = malloc(sizeof(struct ListNode));
    fake_head->val = 0;
    fake_head->next = head;

    struct ListNode* curr_node = head;
    struct ListNode* prev = fake_head;
    while (curr_node) {
        if (val == curr_node->val) {
            prev->next = curr_node->next;
        } else {
            prev = prev->next;
        }
        curr_node = curr_node->next;
    }
    return fake_head->next;
}
```
[返回首页](../README.md)