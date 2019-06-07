# 添加与搜索单词 - 数据结构设计
[返回首页](../README.md)

设计一个支持以下两种操作的数据结构：
```
void addWord(word)
bool search(word)
```
> `search(word)`可以搜索文字或正则表达式字符串，字符串只包含字母`.`或`a-z`。
> 
> `.`可以表示任何一个字母。此处比较麻烦，要保存所有可能。

最简单的办法是用Hash表，理论上查找效率为Θ(1)，但Hash表如果遇到冲突会严重影响效率，所以这种字符串搜索应该用[**Trie树**](https://baike.baidu.com/item/字典树/9825209?fr=aladdin#5_2)。它的特点就是从根到某个节点，把路径上的字符连起来就是要查找的字符串，用空间换时间，插入和搜索效率都是Θ(n)，通常高于哈希表。<br>![Trie tree](https://odhyan.com/blog/wp-content/uploads/2010/11/trie-example.png)<br>根据题目要求，只需要实现插入和搜索两个函数即可。

> 注意：Trie不一定是二叉树
## Python
用dict实现。
```python
class WordDictionary:
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = {}

    def addWord(self, word: str) -> None:
        """
        Adds a word into the data structure.
        """
        node = self.root
        for c in word:
            # 如果这是当前路径下新的字符，则创建一个{c: {}}的字典，否则返回当前itme
            node = node.setdefault(c, {})
        node['$'] = None  # 单词结束标志

    def search(self, word: str) -> bool:
        """
        Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter.
        """
        # 因为要支持通配符，所以这里必须保存所有可能
        nodes = [self.root]

        # 1、找到当前层次的k-v对，赋给node；
        # 2、在node中查找key，如果c在node的key中，则返回node[c]，否则返回第三步结果
        # 3、如果c == '.'，那么返回所有不为None的value，否则返回[]
        # 4、将查找到的item保存至nodes
        for c in '{}$'.format(word):
            nodes = [k for node in nodes for k in
                     ([node[c]] if c in node else filter(None, node.values()) if c == '.' else [])]

        # 检查nodes是否为[]，如果为[]说明没找到
        return bool(nodes)
```
---

## C
```c
```
[返回首页](../README.md)