<!--
 * @Author: Hiseh
 * @Date: 2019-10-11 10:40:23
 * @LastEditors: Hiseh
 * @LastEditTime: 2019-10-22 15:54:35
 * @Description: 
 -->
# 词典中最长的单词
[返回首页](../README.md)

猛一看感觉类似[211添加与搜索单词](./211_add_and_search_word.md)，不过更简单些，不需要考虑通配符查找。如果用**Trie树**，每个节点存数组中的一个元素，最后返回最长路径的节点即可。但是题目里说每个单词都是+1关系，如果某个单词的*word<sub>1</sub>~word<sub>n-1</sub>*不是当前节点，则需要在`root`下创建子节点。考虑到这个关系，也可以用`hash`表实现，代码更简洁，更节约内存。

> Trie树的资料这里不赘述了，请自行百度吧
## Python
```python
class Solution:
    def longestWord(self, words: list) -> str:
        valid = set([""])

        # 用元素长度排序
        for word in sorted(words, key=len):
            if word[:-1] in valid:
                # 如果子字符串在set里，则增加当前单词
                valid.add(word)
        
        # 返回set中最长的单词
        return max(sorted(valid), key=len)
```
---

## C
借用Python思路，用C重写即可。
```c
#include <stdlib.h>
#include <string.h>

#include "uthash.h"

typedef struct {
    char* word;
    UT_hash_handle hh;
} words_struct;
words_struct* words_h = NULL;

int compare(const void* a, const void* b) {
    const char* pa = *(const char**)a;
    const char* pb = *(const char**)b;

    return strcmp(pa, pb);
}

int compare_h(words_struct* p1, words_struct* p2) {
    return -(strlen(p1->word) - strlen(p2->word));
}

char* longestWord(char** words, int wordsSize) {
    qsort(words, wordsSize, sizeof(const char*), compare);
    words_struct* tmp;
    int sign = 0;
    for (int i = 0; i < wordsSize; i++) {
        char* word = words[i];
        int word_length = strlen(word);
        if (word_length == 1) {
            sign = 1;
        } else {
            char sub_word[word_length];
            memcpy(sub_word, word, word_length - 1);
            sub_word[word_length - 1] = '\0';
            HASH_FIND_STR(words_h, sub_word, tmp);
            sign = tmp == NULL ? 0 : 1;
        }

        if (sign == 1) {
            tmp = (words_struct*)malloc(sizeof(words_struct));
            tmp->word = word;
            HASH_ADD_KEYPTR(hh, words_h, tmp->word, strlen(tmp->word), tmp);
        }
    }
    HASH_SORT(words_h, compare_h);
    char* result = words_h->word;

    words_struct* curr_word;

    HASH_ITER(hh, words_h, curr_word, tmp) {
        HASH_DEL(words_h, curr_word);
        free(curr_word);
    }

    return result;
}
```
[返回首页](../README.md)