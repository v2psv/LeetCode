## Problem
You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

**Example 1:**

`coins = [1, 2, 5], amount = 11`

`return 3 (11 = 5 + 5 + 1)`

**Example 2:**

`coins = [2], amount = 3`

`return -1.`

**Note:**

You may assume that you have an infinite number of each kind of coin.

## Code
DP 问题，时间复杂度 **O(mn)**
```java
public class Solution {
    public int coinChange(int[] coins, int amount) {
        Arrays.sort(coins);
        int[] dp = new int[Math.max(amount+2,coins[coins.length-1]+2)];
        for (int i = 0; i < amount+1; i++)
            dp[i] = Integer.MAX_VALUE-100;
        dp[0]  = 0;
        for (int i = 0; i < coins.length; i++)
            dp[coins[i]] = 1;
        
        for (int i = 1; i < amount+1; i++){
            for (int j = 0; j < coins.length && coins[j] < i; j++){
                if (dp[i] > dp[i-coins[j]] + 1)
                    dp[i] = dp[i-coins[j]] + 1;
            }
        }
        
        if (dp[amount] == Integer.MAX_VALUE-100)
            return -1;
        else
            return dp[amount];
    }
}
```
