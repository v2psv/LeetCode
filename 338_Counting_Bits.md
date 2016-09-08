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
先求每 2 个比特中 1 的数量，再求每 4 个比特中 1 的数量，再求每 8 个比特中 1 的数量，再到 16、32 个比特中 1 的数量。

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
- `n -= (n>>>1) & 0x55555555;` 等价于 `n = (n & 0x55555555) + ((n>>>1) & 0x55555555);`，求相邻两个比特中 1 的个数；
- `n = (n & 0x33333333) + ((n>>>2) & 0x33333333);` 同前面的方法；
- 因为 4 个比特中 1 的个数不大于 `0b0100`，故求和不需要考虑进位，`n + (n>>>4)` 求 8 个比特中 1 的个数；
- 4 字节整数 `A B C D * 0x01010101 = (A+B+C+D) (B+C+D) (C+D) D` 

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

# Dynamic Programming
- 若 `i` 是偶数，则 `result[i]=result[i/2]`，即一个偶数 `i` 的二进制中 `1` 的个数与 `2i` 的相同 （一个数的两倍相当于二进制左移一位）
- 若 `i` 是奇数，相当于 `i-1` 中 `1` 的个数加 1
```java
public class Solution {
    public int[] countBits(int num) {
        int[] result = new int[num+1];
        for (int i = 1; i <= num; i++){
            result[i] = result[i/2] + i%2;
        }
        return result;
    }
}
```

# Reference
https://leetcode.com/problems/counting-bits/

http://stackoverflow.com/questions/109023/how-to-count-the-number-of-set-bits-in-a-32-bit-integer

http://www.cnblogs.com/Martinium/archive/2013/03/01/popcount.html
