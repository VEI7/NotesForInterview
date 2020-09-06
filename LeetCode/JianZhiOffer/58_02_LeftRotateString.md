# 左旋转字符串

### Problem

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。
请定义一个函数实现字符串左旋转操作的功能。比如输入字符串"abcdefg"和数字2，该函数将返回左旋转2位得到的结果"cdefgab"。

### Solution

```c++
void Reverse(char *pBegin, char *pEnd)
{
    if(pBegin==nullptr || pEnd==nullptr)
        return;
    while(pBegin<pEnd)
    {
        char temp = *pBegin;
        *pBegin = *pEnd;
        *pEnd = temp;
        pBegin++;
        pEnd--;
    }
}
char* LeftRotateString(char* pStr, int n)
{
    if(pStr!=nullptr)
    {
        int sLength = static_cast<int>(strlen(pStr));
        if(sLength>0 && n>0 && sLength>n)
        {
            char* pFirstStart = pStr;
            char* pFirstEnd = pStr+n-1;
            char* pSecondStart = pStr+n;
            char* pSecondEnd = pStr+sLength-1;
            
            Reverse(pFirstStart, pFirstEnd);// 翻转字符串的前面n个字符
            Reverse(pSecondStart, pSecondEnd);// 翻转字符串的后面部分
            Reverse(pFirstStart, pSecondEnd);// 翻转整个字符串
        }
    }
    return pStr;
}

```





