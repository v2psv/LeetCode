# Dynamic Programming
## 121. Best Time to Buy and Sell Stock
### Problem
Say you have an array for which the `ith` element is the price of a given stock on day `i`.

If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

### Code #1
**状态**

`maxProfit` 当前时间的最大收益，需要考虑前一个时间点的最大收益、当前价格与之前的最小价格之差

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

`pMin` 从左到右顺序的最小价格

`pMax` 从右到左顺序的最大价格

`maxProfit` 当前时间的最大收益等于当前时间之前的最小价格减去当前时间之后的最大价格

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
