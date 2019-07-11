# 叶子相似的树
[返回首页](../README.md)

思路很简单，就是遍历二叉树，然后把所有叶子结点的值拼在一起比较即可。
## Python
遍历二叉树最方便的思路就是用递归，这里采用中遍历方式。因为题目里说了，节点不超过100个，所以没必要考虑内存溢出问题，直接返回数组即可。
```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def leafSimilar(self, root1: TreeNode, root2: TreeNode) -> bool:
        def dfs_(root) -> list:
            if not root:
                return []

            if not root.left and not root.right:
                return [root.val]
            else:
                return dfs_(root.left) + dfs_(root.right)

        return dfs_(root1) == dfs_(root2)
```
---

## C
参照Python思路，用C重写
```c
#include <stdbool.h>
#include <stdlib.h>

struct TreeNode {
    int val;
    struct TreeNode* left;
    struct TreeNode* right;
};

typedef struct dfs_node {
    int val;
    struct dfs_node* next;
} dfs_node;

void dfs(struct TreeNode* root, dfs_node** dfs_list) {
    if (!root) {
        return;
    }

    if (!root->left && !root->right) {
        dfs_node* tmp = (dfs_node*)malloc(sizeof(dfs_node));
        tmp->val = root->val;
        tmp->next = *dfs_list;
        *dfs_list = tmp;
    } else if (root->left) {
        dfs(root->left, dfs_list);
    } else {
        dfs(root->right, dfs_list);
    }
}

bool leafSimilar(struct TreeNode* root1, struct TreeNode* root2) {
    dfs_node* dfs_root1 = NULL;
    dfs_node* dfs_root2 = NULL;
    dfs(root1, &dfs_root1);
    dfs(root2, &dfs_root2);

    bool result = true;
    while (dfs_root1 && dfs_root2) {
        dfs_node* tmp1 = dfs_root1;
        dfs_root1 = dfs_root1->next;
        free(tmp1);

        dfs_node* tmp2 = dfs_root2;
        dfs_root2 = dfs_root2->next;
        free(tmp2);

        if (tmp1->val != tmp2->val) {
            result = false;
            break;
        }
    }

    while (dfs_root1) {
        result = false;
        dfs_node* tmp = dfs_root1;
        dfs_root1 = dfs_root1->next;
        free(tmp);
    }

    while (dfs_root2) {
        result = false;
        dfs_node* tmp = dfs_root2;
        dfs_root2 = dfs_root2->next;
        free(tmp);
    }

    return result;
}
```
[返回首页](../README.md)