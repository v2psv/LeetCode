## Problem
Given a positive integer **n**, break it into the sum of at least two positive integers and maximize the product of those integers. Return the maximum product you can get.

For example, given `n = 2`, return `1 (2 = 1 + 1)`; given `n = 10`, return 36 `(10 = 3 + 3 + 4)`.

**Note:** you may assume that `n` is not less than 2.

> 注意，题目要求返回类型为 int, 所以当 n 超过 59 就会溢出

## Code #1

```java
DP 问题，依次求 n=1,2,3...i 的 最优解。
public class Solution {
    public int integerBreak(int n) {
        if (n <= 3) return (n-1);
        
        int[] dp = new int[n+10];
        for (int i = 0; i < n+1; i++)
            dp[i] = 1;
        
        dp[2] = 2;
        dp[3] = 3;
        
        for (int i = 4; i < n+1; i++){
            for (int j = 1; j < i; j++){
                if (dp[j] * dp[i-j] > dp[i])
                    dp[i] = dp[j] * dp[i-j];
            }
        }
        
        return dp[n];
    }
}
```

## Code #2
当 **n** 大于等于 4 时，切分成 2 或 3 总能使得乘积取最大值。
```java
public class Solution {
    public int integerBreak(int n) {
        int[] dp = new int[n+1];
        if (n <= 3)
            return (n-1);

        dp[1] = 1;
        dp[2] = 2;
        dp[3] = 3;
        for (int i = 4; i < n+1; i++){
            dp[i] = Math.max(dp[i-2]*2, dp[i-3]*3);
        }
        return dp[n];
    }
}
```

## Code #3

```java
public class Solution {
    public int integerBreak(int n) {
        int div = n / 3;
        int mod = n % 3;
        int result = 0;
        
        if (n <= 3)
            return (n-1);
        
        if (mod == 0)
            result = (int)Math.pow(3, div);
        if (mod == 1)
            result = (int)Math.pow(3, div - 1) * 4;
        if (mod == 2)
            result = (int)Math.pow(3, div) * 2;
        return result;
    }
}
```
