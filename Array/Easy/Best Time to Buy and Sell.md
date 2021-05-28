### Question:
You are given an array prices where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a single day to buy one stock and **choosing a different day** in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return `0`.

### Default Thinking:
1. 暴力解法：两两相减，比较profit
2. 想不到第二种解法

### My Solution (did not pass):
```Java
class Solution {
    public int maxProfit(int[] prices) {

        int profit = 0;
        int small = 0;

        for (int base = 0; base < prices.length - 1; base++) {
            if (base == 0) {
                profit = findHigh(prices, base);
                small = prices[base];
            } else {
                if (prices[base] < small) {
                    int tempProfit = findHigh(prices, base);
                    if (tempProfit > profit) {
                        profit = tempProfit;
                        small = prices[base];
                    }
                }
            }
            
        }
        return profit;
    }
    
    public int findHigh(int[] prices, int base) {
        int profit = 0;
        for (int high = base + 1; high < prices.length; high++) {
            int temp = prices[high] - prices[base];
            if (temp > 0) {
                if (temp > profit) {
                    profit = temp;
                }
            }
        }
        return profit;
    }
}
```
* 暴力解法的优化版，但还是time limit exceeded

### Solution from discussion board:
