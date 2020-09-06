# 数组中重复的数字

### Problem

在一个长度为n的数组里的所有数字都在0到n-1的范围内。数组中某些数字是重复的，但不知道有几个数字重复了， 也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。例如，如果输入长度为7的数组{2, 3, 1, 0, 2, 5, 3}，那么对应的输出是重复的数字2或者3。

### Solution1

时间复杂度能达到O(n)，且不用引入额外的存储空间。

```c++
// 参数:
//        numbers:     一个整数数组
//        length:      数组的长度
//        duplication: (输出) 数组中的一个重复的数字
// 返回值:             
//        true  - 输入有效，并且数组中存在重复的数字
//        false - 输入无效，或者数组中没有重复的数字

bool duplicate(int numbers[], int length, int* duplication)
{
    if(numbers == nullptr || length <= 0)
        return false;
    for(int i=0; i<length; i++)
    {
        if(numbers[i]<0 || numbers[i] >=length)
        	return false;
    }
    for(int i=0; i<length; i++)
    {
        while(numbers[i] != i)
        {
            if(numbers[i] == numbers[numbers[i]])
            {
                *duplication = numbers[i];
                return true;
            }
            //swap numbers[i] and numbers[numbers[i]]
            int temp = numbers[i];
            numbers[i] = numbers[temp];
            numbers[temp] = temp;
        }
    }
    return false
}
```

###  要求不改变原数组,在一个长度为n+1的数组里的所有数字都在1到n的范围内

#### solution 1: 空间换时间

新建一个数组，遍历原数组，对于每个元素x，存储到新数组下标为x的位置，很容易找到重复元素。时间复杂度为O(n)，但是空间复杂度为O(n)。

####solution 2: 时间换空间

采用二分查找的方法，时间复杂度为O(nlgn)，空间复杂度为O(1)。且不能找出所有的重复数字。

```c++
// 参数:
//        numbers:     一个整数数组
//        length:      数组的长度
// 返回值:             
//        正数  - 输入有效，并且数组中存在重复的数字，返回值为重复的数字
//        负数  - 输入无效，或者数组中没有重复的数字
int getDuplication(const int* numbers, int length)
{
    if(numbers == nullptr || length <= 0)
        return -1;

    int start = 1;
    int end = length - 1;
    while(end >= start)
    {
        int middle = ((end - start) >> 1) + start;
        int count = countRange(numbers, length, start, middle);
        if(end == start)
        {
            if(count > 1)
                return start;
            else
                break;
        }

        if(count > (middle - start + 1))
            end = middle;
        else
            start = middle + 1;
    }
    return -1;
}

int countRange(const int* numbers, int length, int start, int end)
{
    if(numbers == nullptr)
        return 0;

    int count = 0;
    for(int i = 0; i < length; i++)
        if(numbers[i] >= start && numbers[i] <= end)
            ++count;
    return count;
}

```



