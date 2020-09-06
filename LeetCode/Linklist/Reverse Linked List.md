# 206. Reverse Linked List

###Solution

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head==NULL || head->next==NULL)
            return head;
        ListNode* newHead = NULL;
        ListNode* cur = head;
        
        while(cur)
        {
            ListNode* post = cur->next;
            cur->next = newHead;
            newHead = cur;
            cur = post;
        }
        return newHead;
    }
};


# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def reverseList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        newHead = None
        cur = head
        while cur:
            post = cur.next
            cur.next = newHead
            newHead = cur
            cur = post
        return newHead
```

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head==NULL || head->next==NULL)
            return head;
        ListNode* newHead = reverseList(head->next);
        head->next->next = head;
        head->next = NULL;
        return newHead;
    }
};
```



# 92.Reverse Linked List II

Reverse a linked list from position *m* to *n*. Do it in one-pass.

**Note:** 1 ≤ *m* ≤ *n* ≤ length of list.

**Example:**

```
Input: 1->2->3->4->5->NULL, m = 2, n = 4
Output: 1->4->3->2->5->NULL
```



### Solution

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        ListNode* dummy = new ListNode(-1), *pre = dummy;
        dummy->next = head;
        for(int i=0; i<m-1; i++) pre = pre->next;
        ListNode* last = pre->next, * cur=last->next;
        for(int i=m; i<n; i++)
        {
            last->next = cur->next;
            cur->next = pre->next;
            pre->next = cur;
            cur = last->next;
        }
        return dummy->next;
    }
};
```



# 25. Reverse Nodes in k-Group

Given a linked list, reverse the nodes of a linked list *k* at a time and return its modified list.

*k* is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of *k* then left-out nodes in the end should remain as it is.



**Example:**

Given this linked list: `1->2->3->4->5`

For *k* = 2, you should return: `2->1->4->3->5`

For *k* = 3, you should return: `3->2->1->4->5`

**Note:**

- Only constant extra memory is allowed.
- You may not alter the values in the list's nodes, only nodes itself may be changed.



### Solution

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        if(!head || !head->next || k<2) return head;
        ListNode* dummy = new ListNode(-1), *pre=dummy, *cur=head;
        dummy->next = head;
        for(int i=1; cur; i++)
        {
            if(i%k == 0)
            {
                pre = reverse(pre, cur->next);
                cur = pre->next;
            }
            else
                cur = cur->next;
        }
        return dummy->next;
    }
    
    ListNode* reverse(ListNode* pre, ListNode* next)
    {
        ListNode* last = pre->next, *cur = last->next;
        while(cur != next)
        {
            last->next = cur->next;
            cur->next = pre->next;
            pre->next = cur;
            cur = last->next;
        }
        return last;
    }
    };
```

