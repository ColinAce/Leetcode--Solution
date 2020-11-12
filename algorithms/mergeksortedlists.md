## 题目地址

https://leetcode-cn.com/problems/merge-two-sorted-lists/

## 题目描述
```
给你一个链表数组，每个链表都已经按升序排列。
请你将所有链表合并到一个升序链表中，返回合并后的链表。

示例 1：
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6

示例 2：
输入：lists = []
输出：[]

示例 3：
输入：lists = [[]]
输出：[]
 
提示：
k == lists.length
0 <= k <= 10^4
0 <= lists[i].length <= 500
-10^4 <= lists[i][j] <= 10^4
lists[i] 按 升序 排列
lists[i].length 的总和不超过 10^4
```

## 题目分析
```
方法一:
    用一个变量 ans 来维护以及合并的链表，第 i 次循环把第 i 个链表和 ans 合并，答案保存到 ans 中。(每次两两合并)

方法二：分支合并
    将 k 个链表配对并将同一对中的链表合并；
    第一轮合并以后， k 个链表被合并成了 k/2个链表，平均长度为2n/k，然后是k/4个链表，8/k个链表等等；
    重复这一过程，直到我们得到了最终的有序链表。

```

## 算法
```
分支合并：
    public ListNode mergeKLists(ListNode[] lists) {
        return merge(lists,0,lists.length-1);
    }
    public ListNode merge(ListNode[] lists,int left,int right){
        if(left == right){
            return lists[left];
        }
        else if(left > right){
            return null;
        }
        else{
            int mid = (left + right) >> 1;
            return mergeTwo(merge(lists,left,mid),merge(lists,mid+1,right));
        }
    }
    public ListNode mergeTwo(ListNode a,ListNode b){
        if(a == null || b == null){
            return a != null ? a : b;
        }
        ListNode head = new ListNode(0);
        ListNode tail = head;
        while(a != null && b != null){
            if(a.val <= b.val){
                tail.next = a;
                a = a.next;
            }else{
                tail.next = b;
                b = b.next;
            }
            tail = tail.next;
        }
        tail.next = a != null ? a : b;
        return head.next;
    }
```