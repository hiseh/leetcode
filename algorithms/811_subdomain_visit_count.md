# 统计子域名访问数量
[返回首页](../README.md)

一个网站域名，如"discuss.leetcode.com"，包含了多个子域名。作为顶级域名，常用的有"com"，下一级则有"leetcode.com"，最低的一级为"discuss.leetcode.com"。当我们访问域名"discuss.leetcode.com"时，也同时访问了其父域名"leetcode.com"以及顶级域名 "com"。

给定一个带访问次数和域名的组合，要求分别计算每个域名被访问的次数。其格式为访问次数+空格+地址，例如："9001 discuss.leetcode.com"。

接下来会给出一组访问次数和域名组合的列表cpdomains 。要求解析出所有域名的访问次数，输出格式和输入格式相同，不限定先后顺序。

> **示例**<br/>
> 输入: 
> ["900 google.mail.com", "50 yahoo.com", "1 intel.mail.com", "5 wiki.org"]<br/>
> 输出: 
> ["901 mail.com","50 yahoo.com","900 google.mail.com","5 wiki.org","5 org","1 intel.mail.com","951 com"]<br/>
> 
> **说明:**<br/>
> 按照假设，会访问"google.mail.com" 900次，"yahoo.com" 50次，"intel.mail.com" 1次，"wiki.org" 5次。 而对于父域名，会访问"mail.com" 900+1 = 901次，"com" 900 + 50 + 1 = 951次，和 "org" 5 次。
## Python
使用Hash表，保证每个域名不会被重复计算
```python
class Solution:
    def subdomainVisits(self, cpdomains: list[str]) -> list[str]:
        result = {}
        for domain_v in cpdomains:
            visits, domain = domain_v.split(' ')
            visits = int(visits)

            # 拆分域名
            while '.' in domain:
                result[domain] = result.get(domain, 0) + visits
                # 每个子域名
                domain = domain[domain.index('.')+1:]
            result[domain] = result.get(domain, 0) + visits

        return ['{v} {k}'.format(v=v, k=k) for k, v in result.items()]
```
---

## C
思路同Python。
```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */

#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// hash table用的uthash，在leetcode里已经默认导入这个库，提交答案时不要再导入了。
#include "uthash.h"

typedef struct {
    char* domain;
    int visit;
    UT_hash_handle hh;
} result_h;
result_h* result = NULL;

char** subdomainVisits(char** cpdomains, int cpdomainsSize, int* returnSize) {
    *returnSize = 0;
    for (int i = 0; i < cpdomainsSize; i++) {
        // 访问数
        char* visit_key = " ";
        int visit_i = strcspn(cpdomains[i], visit_key);
        char visit_s[visit_i + 1];

        // leetcode没有ctrlcpy函数，用strncpy代替，注意自己加上'\0'
        strncpy(visit_s, cpdomains[i], visit_i);
        visit_s[visit_i] = '\0';
        int visit = strtol(visit_s, NULL, 10);

        // 域名
        char* domain = cpdomains[i] + visit_i + 1;

        // 遍历子域名组合
        for (int sub_i = 0; sub_i < strlen(domain);
             sub_i = strcspn(domain, ".")) {
            domain = sub_i == 0 ? domain : domain + sub_i + 1;

            // 用hash table过滤
            result_h* tmp = NULL;
            HASH_FIND_STR(result, domain, tmp);
            if (tmp == NULL) {
                tmp = (result_h*)malloc(sizeof(result_h));
                tmp->domain = domain;
                tmp->visit = visit;
                HASH_ADD_KEYPTR(hh, result, tmp->domain, strlen(tmp->domain), tmp);

                (*returnSize)++;
            } else {
                tmp->visit += visit;
            }
        }
    }

    result_h *domain_h, *tmp;

    // 在调用者中释放
    char** return_p = (char**)malloc((*returnSize) * sizeof(char*));
    int i = 0;
    HASH_ITER(hh, result, domain_h, tmp) {
        // 在调用者中释放
        // 最后+2是要加上空格和字符串结尾
        char* result_str = (char*)malloc((floor(log10f(domain_h->visit) + 1) + strlen(domain_h->domain) + 2) * sizeof(char));

        sprintf(result_str, "%d %s", domain_h->visit, domain_h->domain);
        return_p[i++] = result_str;

        // HASH_DEL只删除hash中的key值，不释放
        HASH_DEL(result, domain_h);

        // 释放key
        free(domain_h);
    }
    return return_p;
}
```
[返回首页](../README.md)