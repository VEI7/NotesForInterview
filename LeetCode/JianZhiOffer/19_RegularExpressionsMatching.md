# 正则表达式匹配

### Problem

请实现一个函数用来匹配包含'.'和'表示任意一个字符，而'\*'表示它前面的字符可以出现任意次（含0次）。在本题
中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配与"aa.a"及"ab*a"均不匹配。

### Solution

用有限状态机的思想

```c++
bool match(const char* str, const char* pattern)
{
    if(str == nullptr || pattern == nullptr)
        return false;

    return matchCore(str, pattern);
}

bool matchCore(const char* str, const char* pattern)
{
    if(*str == '\0' && *pattern == '\0')
        return true;

    if(*str != '\0' && *pattern == '\0')
        return false;

    if(*(pattern + 1) == '*')
    {
        if(*pattern == *str || (*pattern == '.' && *str != '\0'))
            // 进入有限状态机的下一个状态
            return matchCore(str + 1, pattern + 2)
            // 继续留在有限状态机的当前状态 
            || matchCore(str + 1, pattern)
            // 略过一个'*' 
            || matchCore(str, pattern + 2);
        else
            // 略过一个'*'
            return matchCore(str, pattern + 2);
    }

    if(*str == *pattern || (*pattern == '.' && *str != '\0'))
        return matchCore(str + 1, pattern + 1);

    return false;
}
```



```c++
class Solution {
public:
    bool isMatch(string s, string p) {
        int m=s.size(), n=p.size();
        vector<vector<bool>> dp(m+1, vector<bool>(n+1, false));
        dp[0][0]=true;
        for(int i=0; i<=m; i++)
        {
            for(int j=1; j<=n; j++)
            {
                if (j > 1 && p[j - 1] == '*') {
                    dp[i][j] = dp[i][j - 2] || (i > 0 && (s[i - 1] == p[j - 2] || p[j - 2] == '.') && dp[i - 1][j]);
                } else {
                    dp[i][j] = i > 0 && dp[i - 1][j - 1] && (s[i - 1] == p[j - 1] || p[j - 1] == '.');
                }
            }
        }
        return dp[m][n];
    }
    
 
};
```

