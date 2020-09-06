# 合并两个排序的链表

### Solution

```c++
ListNode* merge(ListNode* pHead1, ListNode* pHead2)
{
    if(pHead1==nullptr)
        return pHead2;
    if(pHead2==nullptr)
        return pHead1;
    ListNode* mergeHead = nullptr;
    if(pHead1->value <=  pHead2->value)
    {
        mergeHead = pHead1->value;
        mergeHead->pNext = merge(pHead1->pNext, pHead2);
    }
    else
    {
        mergeHead = pHead2->value;
        mergeHead->pNext = merge(pHead1, pHead2->pNext);
    }
    return mergehead;
}
```



