# 问题一：在O(1)时间删除链表结点

### Problem

题目：给定单向链表的头指针和一个结点指针，定义一个函数在O(1)时间删除该结点。

```c++
struct ListNode
{
    int       m_nValue;
    ListNode* m_pNext;
};
```



### Solution

并不需要遍历链表查找并删除该结点，可以直接将下一个结点的内容复制给当前结点，再把下一个结点删除。

```c++
void DeleteNode(ListNode** pListHead, ListNode* pToBeDeleted)
{
    if(!pListHead || !pToBeDeleted)
        return;
    // 要删除的结点不是尾结点
    if(pToBeDeleted->m_pNext != nullptr)
    {
        ListNode* tempNode = pToBeDeleted->m_pNext;
        pToBeDeleted->m_nValue = tempNode->m_nValue;
        pToBeDeleted->m_pNext = tempNode->next;
        delete tempNode;
        tempNode = nullptr;
    }
    // 链表只有一个结点，删除头结点（也是尾结点）
    else if(*pListHead == pToBeDeleted)
    {
        delete pToBeDeleted;
        pToBeDeleted = nullptr;
        *pListHead = nullptr;
    }
    // 链表中有多个结点，删除尾结点
    else
    {
        ListNode* tempNode = *pListHead;
        while(tempNode->m_pNext != pToBeDeleted)
        {
            tempNode = tempNode->m_pNext;
        }
        tempNode->->m_pNext = nullptr;
        delete pToBeDeleted;
        pToBeDeleted = nullptr;
    }
}
```



#问题二：删除链表中重复的结点

### Problem

在一个排序的链表中，如何删除重复的结点？

### Solution

```c++
void DeleteDuplication(ListNode** pHead)
{
    if(pHead == nullptr || *pHead == nullptr)
        return;

    ListNode* pPreNode = nullptr;
    ListNode* pNode = *pHead;
    while(pNode != nullptr)
    {
        ListNode *pNext = pNode->m_pNext;
        bool needDelete = false;
        if(pNext != nullptr && pNext->m_nValue == pNode->m_nValue)
            needDelete = true;

        if(!needDelete)
        {
            pPreNode = pNode;
            pNode = pNode->m_pNext;
        }
        else
        {
            int value = pNode->m_nValue;
            ListNode* pToBeDel = pNode;
            while(pToBeDel != nullptr && pToBeDel->m_nValue == value)
            {
                pNext = pToBeDel->m_pNext;

                delete pToBeDel;
                pToBeDel = nullptr;

                pToBeDel = pNext;
            }

            if(pPreNode == nullptr)
                *pHead = pNext;
            else
                pPreNode->m_pNext = pNext;
            pNode = pNext;
        }
    }
}
```

