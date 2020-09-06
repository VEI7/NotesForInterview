# C++运算符重载

## 赋值运算符函数

#### 给如下类型CMyString的声明，构造赋值运算符函数

```c++
Class CMyString
{
    public:
    	CMyString(char* data=nullptr);
    	CMystring(const CMystring& str);
    	~CMystring(void);
    private:
    	char* m_data;
}
```

需要注意如下几点：

+ 需要把返回值的类型声明为该类型的引用，并在函数结束前返回实例自身的引用*this。只有返回一个引用，才可以进行连续赋值。不然，假设有3个CMyString的对象：str1，str2，str3，在程序中str1=str2=str3将不能通过编译。因为返回值为引用类型的函数某些情况下可作为表达式的左值，而非引用的则一般不可以。
+ 将传入参数的类型声明为常量引用。声明为引用而不是实例是为了减少形参到实参到拷贝构造函数的调用，减少时间开销。声明为const是防止传入的参数被改变。
+ 需要释放实例自身已有的内存
+ 需要判断传入的参数和当前的实例是否为同一个，如果是，直接返回*this。防止释放该实例内存。

```c++
CMyString& CMyString::operator=(const CMyString& str)
{
    if(this == &str)
        return *this;
    
    delete []m_data;
    m_data = nullptr;
    
    m_data = new char[strlen(str.m_data) +1];
    strcpy(m_data, str.m_data);
    return *this;
}
```

#####  进阶版：考虑异常安全性

当内存不足时会导致new char抛出异常，这时m_data是一个空指针。我们可以先创建一个临时实例，再交换临时实例和原来的实例：

```c++
CMyString& CMyString::operator=(const CMyString& str)
{
    if(this == &str)
        return *this;
    
    CMyString strTemp(str);
    char* pTemp = strTemp.m_data;
    strTemp.m_data = m_data;
    m_data = pTemp;
    return *this;
}
```

