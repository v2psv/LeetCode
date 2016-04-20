# Dynamic Programming
## 121. Best Time to Buy and Sell Stock
### Problem
Say you have an array for which the `ith` element is the price of a given stock on day `i`.

If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

### Code #1
**状态**

`minBuy` 每个时间点之前的最小 `Buy` 价格

`maxPro` 每个时间点之前的最大收益

时间复杂度 `O(n)`，空间复杂度 `O(n)`， 时间 `4 ms`

```java
public class Solution {
    public int maxProfit(int[] prices) {
        int length = prices.length;
        if (length <= 1)
            return 0;

        int[] minBuy = new int[length];
        int[] maxPro = new int[length];
        minBuy[0] = prices[0];
        maxPro[0] = 0;
        
        for(int i = 1; i < length; i++) {
            minBuy[i] = Math.min(minBuy[i-1], prices[i]);
            maxPro[i] = Math.max(maxPro[i-1], prices[i]-minBuy[i]);
        }
        
        return maxPro[length-1];
    }
}
```

简化代码，时间复杂度 `O(n)`，空间复杂度 `O(1)`， 时间 `2 ms`

```java
public class Solution {
    public int maxProfit(int[] prices) {
        int length = prices.length;
    
        int minBuy = Integer.MAX_VALUE;
        int maxPro = 0;

        for(int i = 0; i < length; i++) {
            minBuy = Math.min(minBuy, prices[i]);
            maxPro = Math.max(maxPro, prices[i] - minBuy);
        }
        
        return maxPro;
    }
}
```

### Code #2
**状态**

`pMin` 每个时间点之前的最小价格

`pMax` 每个时间点之后的最大价格

`maxProfit` 某一时间点之后的最大价格减去之前的最小价格（因为必须在 `buy` 之后才能 `sell`）

时间复杂度 `O(n)`，空间复杂度 `O(n)`， 时间 `2 ms`

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

## 123. Best Time to Buy and Sell Stock III
### Problem
Say you have an array for which the `ith` element is the price of a given stock on day `i`.

Design an algorithm to find the maximum profit. You may complete at most two transactions.

**Note:**

You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

### Code #1
**状态**

`forward`  每个时间点之前的最大收益

`backword` 每个时间点之后的最大收益

`maxProfit` 某个时间点之前的最大收益与之后的最大收益之和（因为必须 `sell` 之后才能再次 `buy`）

```java
public class Solution {
    public int maxProfit(int[] prices) {
        int length = prices.length;
        if (length <= 1)
            return 0;

        int[] forward = new int[length];
        int[] backword = new int[length];
        int minForward = prices[0];
        int maxBackword = prices[length-1];
        int maxProfit = 0;
        
        forward[0] = backword[length-1] = 0;
        for (int i = 1; i < length; i++){
            forward[i] = Math.max(forward[i-1], prices[i]-minForward);
            minForward = Math.min(minForward, prices[i]);
        }
        
        for (int i = length-2; i >= 0; i--){
            backword[i] = Math.max(backword[i+1], maxBackword-prices[i]);
            maxBackword = Math.max(maxBackword, prices[i]);
        }
        
        for (int i = 0; i < length; i++){
            maxProfit = Math.max(forward[i]+backword[i], maxProfit);
        }
        
        return maxProfit;
    }
}
```

### Code #2
**状态**

`firstBuy` 当前时间点之前的第一次 `buy` 的最小价格

`firstSell` 当前时间点之前的第一次 `sell` 的最大收益

`secondBuy` 当前时间点之前的第二次 `buy` 的最大余额

`secondSell` 当前时间点之前的第二次 `sell` 的最大收益

时间复杂度 `O(n)`，空间复杂度 `O(n)`， 时间 `2 ms`

```java
public class Solution {
    public int maxProfit(int[] prices) {
        int length = prices.length;
        
        int firstBuy = Integer.MAX_VALUE;
        int firstSell = 0;
        int secondBuy = Integer.MIN_VALUE;
        int secondSell = 0;
        
        for (int i = 0; i < length; i++){
            if (firstBuy > prices[i])
                firstBuy = prices[i];
            if (firstSell < prices[i] - firstBuy)
                firstSell = prices[i] - firstBuy;
            if (secondBuy < firstSell - prices[i])
                secondBuy = firstSell - prices[i];
            if (secondSell < secondBuy + prices[i])
                secondSell = secondBuy + prices[i];
        }
        
        return secondSell;
    }
}
```
