# Table of Contents
- [Question](#question)
- [DP1](#dp1)
- [DP2](#dp2)

# Question <a name="question"></a>
You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

**Example 1:**

`coins = [1, 2, 5], amount = 11`

`return 3 (11 = 5 + 5 + 1)`

**Example 2:**

`coins = [2], amount = 3`

`return -1.`

**Note:**

You may assume that you have an infinite number of each kind of coin.

# DP1 <a name = "dp1"></a>
```java
public class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] result = new int[amount+1];
        int Max = amount + 1;
        
        Arrays.fill(result, Max);
        result[0] = 0;
        
        for (int i = 1; i <= amount; i++){
            for (int coin : coins){
                if (coin <= i)
                    result[i] = Math.min(result[i], result[i-coin] + 1);
            }
        }
        return result[amount] > amount ? -1 : result[amount];
    }
}


```

# DP2 <a name = "dp2"></a>

```java
public class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] result = new int[amount+1];
        
        int Max = amount + 1;
        Arrays.fill(result, Max);
        result[0] = 0;
        
        for (int coin : coins){
            for (int i = coin; i <= amount; i++){
                result[i] = Math.min(result[i], result[i-coin] + 1);
            }
        }

        return result[amount] > amount ? -1 : result[amount];
    }
}


```
