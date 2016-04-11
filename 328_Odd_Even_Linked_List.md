# Question
Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in **O(1)** space complexity and **O(nodes)** time complexity.

**Example**:

Given `1->2->3->4->5->NULL`,
return `1->3->5->2->4->NULL`.

**Note**:

The relative order inside both the even and odd groups should remain as it was in the input. 
The first node is considered odd, the second node even and so on ...

# NodeList

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode oddEvenList(ListNode head) {
        if (head == null)
            return head;
        ListNode pointer1 = head, pointer2 = head.next, lastOdd = head;
        boolean isOdd = false;
        
        while (pointer2 != null){
            if (!isOdd){
                pointer2 = pointer2.next;
                pointer1 = pointer1.next;
                isOdd = !isOdd;
                continue;
            }
            
            pointer1.next = pointer2.next;
            pointer2.next = lastOdd.next;
            lastOdd.next = pointer2;
            lastOdd = pointer2;
            pointer2 = pointer1.next;
            isOdd = !isOdd;
        }
        return head;
    }
}
```
