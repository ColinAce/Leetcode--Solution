## 题目地址

https://leetcode-cn.com/problems/merge-two-sorted-lists/

## 题目描述
```
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
 
示例：
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

## 题目分析
```
迭代：
    首先，我们设定一个哨兵节点 prehead ，这可以在最后让我们比较容易地返回合并后的链表。我们维护一个 prev 指针，我们需要做的是调整它的 next 指针。然后，我们重复以下过程，直到 l1 或者 l2 指向了 null ：如果 l1 当前节点的值小于等于 l2 ，我们就把 l1 当前的节点接在 prev 节点的后面同时将 l1 指针往后移一位。否则，我们对 l2 做同样的操作。不管我们将哪一个元素接在了后面，我们都需要把 prev 向后移一位。

    在循环终止的时候， l1 和 l2 至多有一个是非空的。由于输入的两个链表都是有序的，所以不管哪个链表是非空的，它包含的所有元素都比前面已经合并链表中的所有元素都要大。这意味着我们只需要简单地将非空链表接在合并链表的后面，并返回合并链表即可。

递归：
    我们可以如下递归地定义两个链表里的 merge 操作（忽略边界情况，比如空链表等）：
    { 
    list1[0]+merge(list1[1:],list2)     list1[0]<list2[0]
    list2[0]+merge(list1,list2[1:])     otherwise
    }
    也就是说，两个链表头部值较小的一个节点与剩下元素的 merge 操作结果合并。

```

## 代码实现
```
迭代：
    class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode prehead = new ListNode(-1);

        ListNode prev = prehead;
        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                prev.next = l1;
                l1 = l1.next;
            } else {
                prev.next = l2;
                l2 = l2.next;
            }
            prev = prev.next;
        }

        // 合并后 l1 和 l2 最多只有一个还未被合并完，我们直接将链表末尾指向未合并完的链表即可
        prev.next = l1 == null ? l2 : l1;

        return prehead.next;
    }
}

递归：
    class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        } else if (l2 == null) {
            return l1;
        } else if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }

    }
}
```

## 复杂度分析
- 迭代
```
 时间复杂度：O(m+n)
 空间复杂度：O(1)
```

- 递归
```
 时间复杂度：O(m+n)
 空间复杂度：O(m+n)  递归调用会用到栈
```