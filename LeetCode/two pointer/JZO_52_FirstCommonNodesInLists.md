# 两个链表的第一个公共结点

### Problem

输入两个链表，找出它们的第一个公共结点。

### Solution

先各自遍历两个链表获取链表的长度，较长的链表先走（长度差）步，两个指针同步走，遇到的第一个指向同一个节点的就是公共结点

```c++
ListNode* FindFirstCommonNode(ListNode *pHead1, ListNode *pHead2)
{
    // 得到两个链表的长度
    unsigned int nLength1 = GetListLength(pHead1);
    unsigned int nLength2 = GetListLength(pHead2);
    int nLengthDif = nLength1 - nLength2;

    ListNode* pListHeadLong = pHead1;
    ListNode* pListHeadShort = pHead2;
    if(nLength2 > nLength1)
    {
        pListHeadLong = pHead2;
        pListHeadShort = pHead1;
        nLengthDif = nLength2 - nLength1;
    }

    // 先在长链表上走几步，再同时在两个链表上遍历
    for(int i = 0; i < nLengthDif; ++i)
        pListHeadLong = pListHeadLong->m_pNext;

    while((pListHeadLong != nullptr) &&
        (pListHeadShort != nullptr) &&
        (pListHeadLong != pListHeadShort))
    {
        pListHeadLong = pListHeadLong->m_pNext;
        pListHeadShort = pListHeadShort->m_pNext;
    }

    // 得到第一个公共结点
    ListNode* pFisrtCommonNode = pListHeadLong;

    return pFisrtCommonNode;
}

unsigned int GetListLength(ListNode* pHead)
{
    unsigned int nLength = 0;
    ListNode* pNode = pHead;
    while(pNode != nullptr)
    {
        ++nLength;
        pNode = pNode->m_pNext;
    }

    return nLength;
}
```



