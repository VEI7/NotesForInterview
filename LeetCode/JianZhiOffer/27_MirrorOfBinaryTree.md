# 二叉树的镜像

### Problem

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

### Solution

```c++
void MirrorRecursively(BinaryTreeNode *pNode)
{
    if(pNode==nullptr || (pNode->pLeft==nullptr && pNode->pRight==nullptr))
        return;
    BinaryTreeNode * pTemp = pNode->pLeft;
    pNode->pLeft = pNode->pRight;
    pNode->pRight = pTemp;
    
    if(pNode->pLeft)
        MirrorRecursively(pNode->pLeft);
    if(pNode->pRight)
        MirrorRecursively(pNode->pRight);
    
}
```



