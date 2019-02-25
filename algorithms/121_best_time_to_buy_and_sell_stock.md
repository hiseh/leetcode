# 买卖股票的最佳时机
[返回首页](../README.md)

给定一个数组，它的第`i`个元素是一支给定股票第`i`天的价格。
如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。
> 注意你不能在买入股票前卖出股票。
## Python
思路就是找到`max(prices[j] - prices[i])`（j > i），找到最小值之后的最大值。
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if len(prices) < 2:
            return 0
        
        min_price = prices[0]
        max_profit = 0
        for price in prices:
            if price < min_price:
                # 找到最小值
                min_price = price
            elif price - min_price > max_profit:
                # 找到最小值之后的最大值
                max_profit = price - min_price
        return max_profit
```
---

## C
思路同Python
```c
int maxProfit(int* prices, int pricesSize) {
    if (pricesSize < 2)
        return 0;
    int min_price = prices[0];
    int max_profect = 0;

    for (int i = 0; i < pricesSize; i++) {
        if (prices[i] < min_price) {
            min_price = prices[i];
        } else if (prices[i] - min_price > max_profect) {
            max_profect = prices[i] - min_price;
        }
    }
    return max_profect;
}
```
[返回首页](../README.md)