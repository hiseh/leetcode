# 路径总和
[返回首页](../README.md)
一个典型遍历二叉树的题，注意题目中要求判断从**叶子**到**根**的路径，所以检查时别忘了检查当前节点是否是叶子。别的没什么难度了

## Python
遍历二叉树，最简单的办法就是用递归了。
```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def hasPathSum(self, root: TreeNode, sum: int) -> bool:

        # 直到叶子也未找到
        if not root:
            return False

        sum -= root.val

        # 判断当前叶子结点是否符合节点
        if not root.left and not root.right and sum == 0:
            return True

        # 递归遍历
        return self.hasPathSum(root.left, sum) or self.hasPathSum(root.right, sum)
```
---

## C
重新实现一遍Python的思路即可
```c
#include <stdbool.h>
#include <stdlib.h>

struct TreeNode {
    int val;
    struct TreeNode* left;
    struct TreeNode* right;
};

bool hasPathSum(struct TreeNode* root, int sum) {
    if (root == NULL) {
        return false;
    }

    sum -= root->val;
    if (!root->left && !root->right && sum == 0) {
        return true;
    }

    return hasPathSum(root->left, sum) || hasPathSum(root->right, sum);
}
```
[返回首页](../README.md)