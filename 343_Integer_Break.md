**Problem** Given a positive integer n, break it into the sum of at least two positive integers and maximize the product of those integers. Return the maximum product you can get.

For example, given `n = 2`, return `1 (2 = 1 + 1)`; given `n = 10`, return 36 `(10 = 3 + 3 + 4)`.

Note: you may assume that n is not less than 2.

## Code #1
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

## Code #2
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
