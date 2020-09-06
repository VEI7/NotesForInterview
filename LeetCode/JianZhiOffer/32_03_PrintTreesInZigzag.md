# 之字形打印二叉树

### Problem

请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

### Solution

使用两个栈。

```c++
void Print(BinaryTreeNode* pRoot)
{
    if(pRoot == nullptr)
        return;
    stack<BinaryTreeNode*> level[2];
    int current = 0;
    int next = 1;
    level[current].push(pRoot)
    while(!level[current].empty() || !level[next].empty)
    {
        BinaryTreeNode* pNode = level[current].top();
        level[current].pop();
        printf("%d ", pNode->m_nValue);
        if(current == 0)
        {
            if(pNode->pLeft != nullptr)
                level[next].push(pNode->pLeft);
            if(pNode->pRight != nullptr)
                level[next].push(pNode->pRight);
        }
        else
        {
            if(pNode->pRight != nullptr)
                level[next].push(pNode->pRight);
            if(pNode->pLeft != nullptr)
                level[next].push(pNode->pLeft);
        }
        if(level[current].empty())
        {
            printf("\n");
            current = 1-current;
            next = 1-next;
        }
    }
    
}
```



