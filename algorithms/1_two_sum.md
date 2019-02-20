# 两数之和
[返回首页](../README.md)

给定一个整数数组`nums`和一个目标值`target`，请你在该数组中找出和为目标值的那**两个**整数，并返回他们的数组下标。

## Python
将目标元素作为key，元素下标作为value存入dict中，如果遇到匹配的值，返回dict中的value和匹配值的下标即刻。此处用dict，是为了保证不会重复使用相同的元素。一共6行代码，没啥可说的。
```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        result = {}
        for i, x in enumerate(nums):
            y = target - x
            if y in result:
                return i, result[y]
            result[x] = i
```
---

## C
与Python思路一样，麻烦一点的是C中没有现成的hash库，可以用[uthash](https://github.com/troydhanson/uthash)，也可以自己实现一个。
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct HashNode {
    int val;
    int key;
    struct HashNode* next;
} HashNode;

static inline int hash(int val, int n) {
    int index = val % n;
    return (index > 0) ? index : -index;
}

int* twoSum(int numbers[], int n, int target) {
    static int ret[2] = {0, 0};
    int i;

    HashNode** hashtable = (HashNode**)calloc(n, sizeof(HashNode*));

    int idx;
    HashNode *p, *tail;
    for (i = 0; i < n; i++) {
        idx = hash(numbers[i], n);
        p = hashtable[idx];
        tail = NULL;
        while (p != NULL) {
            tail = p;
            p = p->next;
        }
        HashNode* new_node = (HashNode*)calloc(1, sizeof(HashNode));
        new_node->val = numbers[i];
        new_node->key = i;
        new_node->next = NULL;

        if (tail) {
            tail->next = new_node;
        } else {
            hashtable[idx] = new_node;
        }
    }

    for (i = 0; i < n; i++) {
        int diff = target - numbers[i];
        idx = hash(diff, n);
        p = hashtable[idx];
        while (p != NULL) {
            if (p->val == diff && p->key != i) {
                ret[0] = p->key;
                ret[1] = i;
                if (ret[0] > ret[1]) {
                    ret[0] = ret[0] ^ ret[1];
                    ret[1] = ret[0] ^ ret[1];
                    ret[0] = ret[0] ^ ret[1];
                }
                return ret;
            }
            p = p->next;
        }
    }
    return ret;
}
```
[返回首页](../README.md)