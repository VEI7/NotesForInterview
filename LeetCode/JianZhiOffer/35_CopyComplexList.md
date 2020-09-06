# 复杂链表的复制

### Problem

请实现函数ComplexListNode* Clone(ComplexListNode* pHead)，复制一个复杂链表。在复杂链表中，每个结点除了有一个m_pNext指针指向下一个结点外，还有一个m_pSibling 指向链表中的任意结点或者nullptr。

### Solution

```c++
struct ComplexListNode
{
    int                 m_nValue;
    ComplexListNode*    m_pNext;
    ComplexListNode*    m_pSibling;
};
void CloneNodes(ComplexListNode* pHead);
void ConnectSiblingNodes(ComplexListNode* pHead);
ComplexListNode* ReconnectNodes(ComplexListNode* pHead);

ComplexListNode* Clone(ComplexListNode* pHead)
{
    CloneNodes(pHead);
    ConnectSiblingNodes(pHead);
    return ReconnectNodes(pHead);
}

void CloneNodes(ComplexListNode* pHead)
{
    ComplexListNode* pNode = pHead;
    while(pNode!=nullptr)
    {
        ComplexListNode* pCloned = new ComplexListNode();
        pCloned-> m_nValue = pNode-> m_nValue;
        pCloned-> m_pNext = pNode-> m_pNext;
        pCloned-> m_pSibling = nullptr;
        pNode-> m_pNext = pCloned;
        pNode = pCloned->m_pNext;
    }
}

void ConnectSiblingNodes(ComplexListNode* pHead)
{
    ComplexListNode* pNode = pHead;
    while(pNode!=nullptr)
    {
        ComplexListNode* pCloned = pNode->m_pNext;
        if(pNode->m_pSibling != nullptr)
        {
            pCloned->m_pSibling = pNode->m_pSibling->m_pNext;
        }
        pNode = pCloned->m_pNext;
    }
}

ComplexListNode* ReconnectNodes(ComplexListNode* pHead)
{
    ComplexListNode* pNode = pHead;
    ComplexListNode* pClonedHead = nullptr;
    ComplexListNode* pClonedNode = nullptr;
    if(pNode != nullptr)
    {
        pClonedHead = pClonedNode = pNode->m_pNext;
        pNode->m_pNext = pClonedNode->m_pNext;
        pNode = pNode->m_pNext;
    }
    while(pNode != nullptr)
    {
        pClonedNode->m_pNext = pNode->m_pNext;
        pClonedNode = pClonedNode->m_pNext;
        
        pNode->m_pNext = pClonedNode->m_pNext;
        pNode = pNode->m_pNext;
    }
    return pClonedHead;
}
```



