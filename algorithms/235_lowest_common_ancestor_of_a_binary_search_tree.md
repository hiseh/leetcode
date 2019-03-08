# 二叉搜索树的最近公共祖先
[返回首页](../README.md)

如果对[二叉搜索树(BST)](https://baike.baidu.com/item/二叉搜索树/7077855?fr=aladdin)不清楚，建议先看下定义。通俗地说就是若左子树不空，则左子树上所有结点的值均小于根结点的值；若右子树不空，则右子树上所有结点的值均大于根结点的值；此外左、右子树也分别为二叉搜索树。单纯的二叉搜索树容易左右不平衡，效率并不很高。通常我们用**红黑树**（二叉搜索树及二叉平衡术结合体）来实现set/map等算法，Linux中也用来做内存管理。

这道题就是一个典型LCA问题，一般这种问题解决办法是递归或者循环。递归算法非常好理解，利用BST左右子树值大小的特点实现，优化成尾递归后效率也不是大问题。

## Python
```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode',
                             q: 'TreeNode') -> 'TreeNode':
        if root is None:
            return
            
        if p.val <= root.val <= q.val or p.val >= root.val >= q.val:
            return root
        elif p.val > root.val and q.val > root.val:
            return self.lowestCommonAncestor(root.right, p, q)
        elif p.val < root.val and q.val < root.val:
            return self.lowestCommonAncestor(root.left, p, q)
```
---

## C
```c
#include <stdlib.h>

struct TreeNode {
    int val;
    struct TreeNode* left;
    struct TreeNode* right;
};

struct TreeNode* lowestCommonAncestor(struct TreeNode* root,
                                      struct TreeNode* p,
                                      struct TreeNode* q) {
    if (root == NULL) {
        return;
    }

    if (p->val > root->val && q->val > root->val) {
        return lowestCommonAncestor(root->right, p, q);
    } else if (p->val < root->val && q->val < root->val) {
        return lowestCommonAncestor(root->left, p, q);
    } else {
        return root;
    }
}
```
[返回首页](../README.md)