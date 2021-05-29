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
1. Kadane's Algorithm 
```Java
public int maxProfit(int[] prices) {
    int maxCur = 0, maxSoFar = 0;
    for(int i = 1; i < prices.length; i++) {
        maxCur = Math.max(0, maxCur += prices[i] - prices[i-1]);
        maxSoFar = Math.max(maxCur, maxSoFar);
    }
    return maxSoFar;
}
```
*maxCur = current maximum value

*maxSoFar = maximum value found so far

##### 解释一下maxCur += prices[i] - prices[i-1]的逻辑：
```
A more clear explanation on why sum of subarray works.:

Suppose we have original array:
[a0, a1, a2, a3, a4, a5, a6]

what we are given here(or we calculate ourselves) is:
[b0, b1, b2, b3, b4, b5, b6]

where,
b[i] = 0, when i == 0
b[i] = a[i] - a[i - 1], when i != 0

suppose if a2 and a6 are the points that give us the max difference (a2 < a6)
then in our given array, we need to find the sum of sub array from b3 to b6.

b3 = a3 - a2
b4 = a4 - a3
b5 = a5 - a4
b6 = a6 - a5

adding all these, all the middle terms will cancel out except two
i.e.

b3 + b4 + b5 + b6 = a6 - a2

a6 - a2 is the required solution.

so we need to find the largest sub array sum to get the most profit
```
* Time Complexity: O(N)
* Space Complexity: O(1)

2. Math.max
```Java
public int maxProfit(int[] prices) {
    if (prices.length == 0) {
        return 0 ;
    }		
    int max = 0 ;
    int sofarMin = prices[0] ;
    for (int i = 0 ; i < prices.length ; ++i) {
        if (prices[i] > sofarMin) {
            max = Math.max(max, prices[i] - sofarMin) ;
        } else{
            sofarMin = prices[i];  
        }
    }	     
    return  max ;
}
```
* Time Complexity: O(N)
* Space Complexity: O(1)

### 反思

1. 不管怎么说
