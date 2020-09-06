# 对称的二叉树

### Problem

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

### Solution

```c++
bool isSymmetrical(BinaryTreeNode* pRoot1, BinaryTreeNode* pRoot2);

bool isSymmetrical(BinaryTreeNode* pRoot)
{
    return isSymmetrical(pRoot, pRoot);
}

bool isSymmetrical(BinaryTreeNode* pRoot1, BinaryTreeNode* pRoot2)
{
    if(pRoot1==nullptr && pRoot2==nullptr)
        return true;
    if(pRoot1==nullptr || pRoot2==nullptr)
        return true;
    if(pRoot1->value != pRoot2->value)
        return false;
    
    return isSymmetrical(pRoot1->pLeft, pRoot2->pRight) 
        && isSymmetrical(pRoot1->pRight, pRoot2->pLeft);
}
```



