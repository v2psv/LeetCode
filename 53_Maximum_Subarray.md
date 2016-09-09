# Table of Contents
- [Question](#question)
- [DP1](#dp1)
- [DP2](#dp2)

# Question <a name = "question"></>
Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

For example, given the array `[-2,1,-3,4,-1,2,1,-5,4]`,
the contiguous subarray `[4,-1,2,1]` has the largest `sum = 6`.

# DP1 <a name = "dp1"></a>
`dp` 记录以当前元素最为最后一个元素的最大和
空间 `O(n)`, 时间 O(n)
```java
public class Solution {
    public int maxSubArray(int[] nums) {
        int max, dp;
        
        dp = max = nums[0];
        for (int i = 1; i < nums.length; i++){
            dp = Math.max(nums[i], nums[i]+dp);
            max = Math.max(max, dp);
        }
        return max;
    }
}
```

# DP2 <a name = "dp2"></a>
空间 `O(1)`, 时间 O(n)
```java
public class Solution {
    public int maxSubArray(int[] nums) {
        int max, dp;
        
        dp = max = nums[0];
        for (int i = 1; i < nums.length; i++){
            dp = Math.max(nums[i], nums[i]+dp);
            max = Math.max(max, dp);
        }
        return max;
    }
}
```
