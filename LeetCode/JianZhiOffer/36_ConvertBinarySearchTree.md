# 二叉搜索树与双向链表

### Problem

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

### Solution

在中序遍历递归过程中，把已经转换为排序双向链表的左右子树与根节点连接起来。

```c++
void ConvertNode(BinaryTreeNode* pNode, BinaryTreeNode** pLastNodeInList);

BinaryTreeNode* Convert(BinaryTreeNode* pRootOfTree)
{
    BinaryTreeNode *pLastNodeInList = nullptr;
    ConvertNode(pRootOfTree, &pLastNodeInList);

    // pLastNodeInList指向双向链表的尾结点，
    // 我们需要返回头结点
    BinaryTreeNode *pHeadOfList = pLastNodeInList;
    while(pHeadOfList != nullptr && pHeadOfList->m_pLeft != nullptr)
        pHeadOfList = pHeadOfList->m_pLeft;

    return pHeadOfList;
}

void ConvertNode(BinaryTreeNode* pNode, BinaryTreeNode** pLastNodeInList)
{
    if(pNode==nullptr)
        return;
    BinaryTreeNode* pCurrent = pNode;
    if(pCurrent->m_pLeft!=nullptr)
        ConvertNode(pCurrent->m_pLeft, pLastNodeInList);
    
    pCurrent->m_pLeft = *pLastNodeInList;
    if(*pLastNodeInList != nullptr)
        *pLastNodeInList->m_pRight = pCurrent;
    *pLastNodeInList = pCurrent;
  	
    if(pCurrent->m_pRight!=nullptr)
        ConvertNode(pCurrent->m_pRight, pLastNodeInList);
}


```



