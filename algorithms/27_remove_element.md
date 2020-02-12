<!--
 * @Author: Hiseh
 * @Date: 2020-02-11 16:16:33
 * @LastEditors  : Hiseh
 * @LastEditTime : 2020-02-12 15:43:56
 * @Description: 移除元素 
 -->
# 移除元素
[返回首页](../README.md)

这道题本身很好理解，就是去掉给定的元素，返回新数组长度。不过有一个约束条件需要动动脑子：内存要**in-place**，不能申请新内存。<br/>
## Python
`remove`方法就是*in-place*的，python如此这么简单。
```python
class Solution:
    def removeElement(self, nums: list, val: int) -> int:
        while True:
            try:
                nums.remove(val)
            except ValueError:
                break
        return len(nums)
```
---

## C
C没有现成函数，分析Python的`remove`函数代码：
```c
static PyObject *
list_remove(PyListObject *self, PyObject *value) {
    Py_ssize_t i;

    for (i = 0; i < Py_SIZE(self); i++) {
        int cmp = PyObject_RichCompareBool(self->ob_item[i], value, Py_EQ);
        if (cmp > 0) {
            if (list_ass_slice(self, i, i+1,
                               (PyObject *)NULL) == 0)
                Py_RETURN_NONE;
            return NULL;
        }
        else if (cmp < 0)
            return NULL;
    }
    PyErr_SetString(PyExc_ValueError, "list.remove(x): x not in list");
    return NULL;
}
```
逻辑很简单，因为只删掉第一个符合条件的字符，所以删掉后直接把后面字符补上去。其实思路是双指针思路：遇到符合条件的元素时，最后一个元素复制到当前位置，并释放最后一个元素。
> btw，`list_ass_slice`函数用递归实现，怪不得大数组下这么慢呢
```c
int removeElement(int* nums, int numsSize, int val) {
    for (int i = 0; i < numsSize; i++) {
        if (nums[i] == val) {
            // i < --numsSize  说明不是最后一个元素==val 
            // numsSize > 0  说明不是所有的元素都==val
            while (i < --numsSize && numsSize > 0) {
                // 只把!=val的元素复制到nums[i]处
                if (nums[numsSize] != val) {
                    nums[i] = nums[numsSize];
                    break;
                }
            }
        }
    }

    return numsSize;
}
```
[返回首页](../README.md)