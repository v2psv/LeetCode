# Question
Given a non negative integer number num. For every numbers i in the range 0 ≤ i ≤ num calculate the number of 1's in their binary representation and return them as an array.

**Example:**
`For num = 5 you should return [0,1,1,2,1,2].`

**Follow up:**

It is very easy to come up with a solution with run time **O(n*sizeof(integer))**. But can you do it in linear time **O(n) /possibly** in a single pass?
Space complexity should be **O(n)**.
Can you do it like a boss? Do it without using any builtin function like **__builtin_popcount** in c++ or in any other language.

# Code

```
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

# Reference
https://leetcode.com/problems/counting-bits/

http://stackoverflow.com/questions/109023/how-to-count-the-number-of-set-bits-in-a-32-bit-integer

http://www.cnblogs.com/Martinium/archive/2013/03/01/popcount.html
