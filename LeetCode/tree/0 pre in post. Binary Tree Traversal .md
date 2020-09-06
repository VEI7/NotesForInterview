# 94. Binary Tree Inorder Traversal

###Problem

Given a binary tree, return the *inorder* traversal of its nodes' values.

**Example:**

```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,3,2]
```

**Follow up:** Recursive solution is trivial, could you do it iteratively?

###Solution

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def inorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        res = []
        stack = []
        p = root
        while p or len(stack):
            while p:
                stack.append(p)
                p = p.left
            p = stack[-1]
            stack.pop()
            res.append(p.val)
            p = p.right
        return res
            
```



```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def inorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        res = []
        
        def inorder(node):
            if not node:
                return
            inorder(node.left)
            res.append(node.val)
            inorder(node.right)
        
        inorder(root)
        return res
            
```



##144.Binary Tree Preorder Traversal

Given a binary tree, return the *preorder* traversal of its nodes' values.

**Example:**

```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,2,3]
```

**Follow up:** Recursive solution is trivial, could you do it iteratively?



```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def preorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if not root:
            return []
        res = []
        stack = [root]
        while len(stack):
            node = stack[-1]
            res.append(node.val)
            stack.pop()
            if node.right:
                stack.append(node.right)
            if node.left:
                stack.append(node.left)
        return res
            
```



```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def preorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        res = []
        stack = []
        p=root
        while p or len(stack):
            if p:
                res.append(p.val)
                stack.append(p)
                p=p.left
            else:
                p = stack[-1]
                stack.pop()
                p=p.right
        return res
            
```



## 145.Binary Tree Postorder Traversal

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def postorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if not root:
            return []
        stack = [root]
        head = root
        res = []
        while stack:
            t = stack[-1]
            if (not t.left and not t.right) or t.left == head or t.right == head:
                head = t
                res.append(t.val)
                stack.pop()
            else:
                if t.right:
                    stack.append(t.right)
                if t.left:
                    stack.append(t.left)
        return res
```

