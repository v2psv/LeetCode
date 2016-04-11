# Question
Given a non negative integer number **num**. For every numbers i in the range **0 ≤ i ≤ num** calculate the number of 1's in their binary representation and return them as an array.

**Example:**
For `num = 5` you should `return [0,1,1,2,1,2]`.

**Follow up:**

It is very easy to come up with a solution with run time **O(n*sizeof(integer))**. But can you do it in linear time **O(n) /possibly** in a single pass?
Space complexity should be **O(n)**.
Can you do it like a boss? Do it without using any builtin function like **__builtin_popcount** in c++ or in any other language.

# 查找表
首先查找表列出 8 位字节所有的可能（2^8），将 32 位 int 拆成 4 个字节处理，再逐个对整数的每个字节在查找表中找到 1 的个数。

```java
public class Solution {
    private static int[] LOOKUP = new int[] { 
        0, 1, 1, 2, 1, 2, 2, 3, 1, 2, 2, 3, 2, 3, 3, 4, // 0n
        1, 2, 2, 3, 2, 3, 3, 4, 2, 3, 3, 4, 3, 4, 4, 5, // 1n
        1, 2, 2, 3, 2, 3, 3, 4, 2, 3, 3, 4, 3, 4, 4, 5, // 2n
        2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6, // 3n
        1, 2, 2, 3, 2, 3, 3, 4, 2, 3, 3, 4, 3, 4, 4, 5, // 4n
        2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6, // 5n
        2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6, // 6n
        3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7, // 7n
        1, 2, 2, 3, 2, 3, 3, 4, 2, 3, 3, 4, 3, 4, 4, 5, // 8n
        2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6, // 9n
        2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6, // An
        3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7, // Bn
        2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6, // Cn
        3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7, // Dn
        3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7, // En
        4, 5, 5, 6, 5, 6, 6, 7, 5, 6, 6, 7, 6, 7, 7, 8  // Fn
    };

    public int[] countBits(int num) {
        int[] count = new int[num + 1];

        for (int i = 0; i <= num; i++) {
            count[i] += LOOKUP[i & 0xff];
            count[i] += LOOKUP[(i >>> 8) & 0xff];
            count[i] += LOOKUP[(i >>> 16) & 0xff];
            count[i] += LOOKUP[(i >>> 24) & 0xff];
        }

        return count;
    }
}
```

# Popcount
相邻两个比特相加得到这两个比特 1 的数量，得到 16 组和，再两两相加...

```
01010101 01010101 01010101 01010101 = 0x55555555
00110011 00110011 00110011 00110011 = 0x33333333
00001111 00001111 00001111 00001111 = 0x0F0F0F0F
00000000 11111111 00000000 11111111 = 0x00FF00FF
00000000 00000000 11111111 11111111 = 0x0000FFFF
```

```java
public class Solution {
    public int[] countBits(int num) {
        int[] count = new int[num+1];
        
        for (int i = 0; i <= num; i++){
            int n = i;
            n = (n & 0x55555555) + ((n>>>1) & 0x55555555);
            n = (n & 0x33333333) + ((n>>>2) & 0x33333333);
            n = (n & 0x0F0F0F0F) + ((n>>>4) & 0x0F0F0F0F);
            n = (n & 0x00FF00FF) + ((n>>>8) & 0x00FF00FF);
            n = (n & 0x0000FFFF) + ((n>>>16)& 0x0000FFFF);
            
            count[i] = n;
        }
        
        return count;
    }
}
```

# Optimized Popcount

```java
public class Solution {
    public int[] countBits(int num) {
        int[] count = new int[num+1];
        
        for (int i = 0; i <= num; i++){
            int n = i;
            n -= (n>>>1) & 0x55555555;
            n = (n & 0x33333333) + ((n>>>2) & 0x33333333);
            count[i] = (((n + (n>>>4)) & 0x0F0F0F0F) * 0x01010101) >>> 24;
        }
        
        return count;
    }
}
```
# Reference
https://leetcode.com/problems/counting-bits/

http://stackoverflow.com/questions/109023/how-to-count-the-number-of-set-bits-in-a-32-bit-integer

http://www.cnblogs.com/Martinium/archive/2013/03/01/popcount.html
