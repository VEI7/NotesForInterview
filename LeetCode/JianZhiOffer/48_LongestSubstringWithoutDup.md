# 最长不含重复字符的子字符串

### Problem

请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。假设字符串中只包含从'a'到'z'的字符。

### Solution

```c++
int longestSubstringWithoutDuplication(const std::string& str)
{
    int curLength = 0;
    int maxLength = 0;

    int* position = new int[26];
    for(int i = 0; i < 26; ++i)
        position[i] = -1;
    
    for(int i=0; i<str.length(); i++)
    {
        int preIndex = position[str[i] - 'a'];
        if(preIndex<0 || i-preIndex>curLength)
            curLength++;
        else
        {
            if(curLength>maxLength)
                maxLength=curLength;
            curLength=i-preIndex;
        }
        position[str[i] - 'a'] = i;
    }
    if(curLength>maxLength)
        maxLength=curLength;
    delete[] position;
    return maxLength; 
}

```





