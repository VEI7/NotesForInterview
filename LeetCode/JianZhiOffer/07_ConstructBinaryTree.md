# 重建二叉树

### Problem

根据前序遍历和中序遍历重建二叉树

### Solution

```c++
BinaryTreeNode* ConstructCore(int* startPreorder, int* endPreorder, int* startInorder, int* endInorder);

BinaryTreeNode* Construct(int* preorder, int* inorder, int length)
{
    if(preorder == nullptr || inorder == nullptr || length <= 0)
        return nullptr;

    return ConstructCore(preorder, preorder + length - 1,
        inorder, inorder + length - 1);
}

BinaryTreeNode* ConstructCore(int* startPreorder, int* endPreorder, int* startInorder, int* endInorder)
{
    int rootValue = startPreorder[0];
    BinaryTreeNode* root = new BinaryTreeNode();
    root->t_value = rootValue;
    root->t_left = root->t_right = nullptr;
    
    if(startPreorder == endPreorder)
    {
        if(startInorder == endInorder && *startPreorder == *startInorder)
            return root;
        else
            throw std::exception("Invalid input.");
    }
    
    int* rootInorder = startInorder;
    while(rootInorder <= enInorder && *rootInorder != rootvalue)
        rootInorder++;
    
    if(rootInorder > enInorder)
        throw std::exception("Invalid input.");
	int leftLength = rootInorder - startInorder;
    int* leftPreorderEnd = startPreorder + leftLength;
    if(leftLength >0)
    {
        root->t_left = ConstructCore(startPreorder+1, leftPreorderEnd, startInorder, rootInorder-1);
    }
    if(leftLength<endPreorder-startPreorder)
    {
        root->t_right = ConstructCore(leftPreorderEnd+1, endPreorder, rootInorder+1, endInorder);
    }
    return root;
}

```

