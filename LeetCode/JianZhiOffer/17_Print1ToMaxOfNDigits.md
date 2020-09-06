# 打印从1到最大的n位数

### Problem

题目：输入数字n，按顺序打印出从1最大的n位十进制数。比如输入3，则打印出1、2、3一直到最大的3位数即999。



#### 注意事项

需要考虑int整数的溢出问题。因此考虑用字符串或数组来模拟整数。

### Solution1

```c++
void Print1ToMaxOfNDigits_1(int n)
{
    if (n <= 0)
        return;

    char *number = new char[n + 1];
    memset(number, '0', n);
    number[n] = '\0';

    while (!Increment(number))
    {
        PrintNumber(number);
    }

    delete[]number;
}

// 字符串number表示一个数字，在 number上增加1
// 如果做加法溢出，则返回true；否则为false
bool Increment(char* number)
{
    bool isOverflow = false;
    int nTakeOver = 0;
    int nLength = strlen(number);

    for (int i = nLength - 1; i >= 0; i--)
    {
        int nSum = number[i] - '0' + nTakeOver;
        if (i == nLength - 1)
            nSum++;

        if (nSum >= 10)
        {
            if (i == 0)
                isOverflow = true;
            else
            {
                nSum -= 10;
                nTakeOver = 1;
                number[i] = '0' + nSum;
            }
        }
        else
        {
            number[i] = '0' + nSum;
            break;
        }
    }

    return isOverflow;
}

// 字符串number表示一个数字，数字有若干个0开头
// 打印出这个数字，并忽略开头的0
void PrintNumber(char* number)
{
    bool isBeginning0 = true;
    int nLength = strlen(number);

    for (int i = 0; i < nLength; ++i)
    {
        if (isBeginning0 && number[i] != '0')
            isBeginning0 = false;

        if (!isBeginning0)
        {
            printf("%c", number[i]);
        }
    }

    printf("\t");
}

```



### Solution2

转换为数字排列的问题：n位以内的所有的十进制数实际上是n个从0到9的全排列。全排列用递归很容易表达，数字每一位都是0-9中的其中一位，然后设置下一位。递归结束的条件是我们已经设置了数字的最后一位。

```c++
void Print1ToMaxOfNDigits_1(int n)
{
    if (n <= 0)
        return;

    char *number = new char[n + 1];
    number[n] = '\0';
	
    for(int i=0; i<=9; i++)
    {
        number[0] = '0'+i;
        recursive(number, n, 0)
    }
    
    delete[]number;
}

void recursive(char* number, int n, int index)
{
    if(index == n-1)
    {
        PrintNumber(number);
        return;
    }
    for(int i=0; i<=9; i++)
    {
        number[index+1] = '0'+i;
        recursive(number, n, index+1);
    }
}

// 字符串number表示一个数字，数字有若干个0开头
// 打印出这个数字，并忽略开头的0
void PrintNumber(char* number)
{
    bool isBeginning0 = true;
    int nLength = strlen(number);

    for (int i = 0; i < nLength; ++i)
    {
        if (isBeginning0 && number[i] != '0')
            isBeginning0 = false;

        if (!isBeginning0)
        {
            printf("%c", number[i]);
        }
    }

    printf("\t");
}

```

