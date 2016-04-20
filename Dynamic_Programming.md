# Dynamic Programming
## 121. Best Time to Buy and Sell Stock
### Problem
Say you have an array for which the `ith` element is the price of a given stock on day `i`.

If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

### Code #1
**状态**

`maxProfit` 某个时间点的最大收益，需要考虑前一个时间点的最大收益、当前价格与之前的最小价格之差

```java
public class Solution {
    public int maxProfit(int[] prices) {
        int length = prices.length;
        
        int minPrice = Integer.MAX_VALUE;
        int maxProfit = 0;
        
        for (int i = 0; i < length; i++){
            maxProfit = Math.max(maxProfit, prices[i]-minPrice);
            minPrice = Math.min(minPrice, prices[i]);
        }
        
        return maxProfit;
    }
}
```

### Code #2
**状态**

`pMin` 每个时间点之前的最小价格

`pMax` 每个时间点之后的最大价格

`maxProfit` 某一时间点之后的最大价格减去之前的最小价格

```java
public class Solution {
    public int maxProfit(int[] prices) {
        int length = prices.length;
        if (length <= 1)
            return 0;

        int[] pMin = new int[length];
        int[] pMax = new int[length];
        int maxProfit = 0;
    
        pMin[0] = prices[0];
        for(int i = 1; i < length; i++) {
            pMin[i] = Math.min(pMin[i-1], prices[i]);
        }
        
        pMax[length-1] = prices[length-1];
        for(int i = length-2; i >= 0; i--) {
            pMax[i] = Math.max(pMax[i+1], prices[i]);
        }

        for(int i = 0; i < length; i++) {
            maxProfit = Math.max(maxProfit, pMax[i] - pMin[i]);
        }
        return maxProfit;
    }
}
```

## 122. Best Time to Buy and Sell Stock II
### Problem
Say you have an array for which the `ith` element is the price of a given stock on day `i`.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

### Code
**状态**

`maxProfit` 某一时间点之前的最大收益

```java
public class Solution {
    public int maxProfit(int[] prices) {
        int length = prices.length;
        int maxProfit = 0;
        
        for (int i = 1; i < length; i++){
            if (prices[i] > prices[i-1])
                maxProfit += prices[i] - prices[i-1];
        }
        return maxProfit;
    }
}
```
