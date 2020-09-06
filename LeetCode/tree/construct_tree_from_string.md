# Construct tree from string

### Problem

给定一个合法的描述树的字符串，字符串转换为树结构，如： A(BC(DE)F)

### Solution

```c++
struct TreeNode{
    char val;
    vector<TreeNode*> children;
    TreeNode(char v) : val(v) {}
};

vector<TreeNode*> transformCore(string& str, int& start)
{
    vector<TreeNode*> res;

    while(start<str.size() && str[start]!=')')
    {
        if(str[start] == '(')
        {
            res.back()->children = transformCore(str, ++start);
        }
        else
        {
            TreeNode* node = new TreeNode(str[start++]);
            res.push_back(node);
        }
    }
    start++;
    return res;
}

TreeNode* Transform(string str)
{
    if(str == "") return nullptr;
    TreeNode* head = new TreeNode(str[0]);
    int start=2;
    head->children = transformCore(str, start);
    return head;
}

void DFS(TreeNode* node)
{
    cout<<node->val;
    if(!node->children.empty())
    {
        cout<<'(';
        for(auto c : node->children)
        {
            DFS(c);
        }
        cout<<')';
    }
}

int main()
{
    string s="A(BC(DE)F)";
    TreeNode* head = Transform(s);
    DFS(head);
    return 0;
}
```





