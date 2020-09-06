# 数值的整数次方

### Problem

题目：实现函数double Power(double base, int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。

### Solution1

```c++
bool g_InvalidInput = false;
double power(double base, int exponent)
{
   	g_InvalidInput = false;
   	
    if(equal(base, 0.0) && exponent<0)
    {
        g_InvalidInput = true;
        return 0.0;
    }
    
    unsigned int absExponent = (unsigned int)(exponent);
    if(exponent<0)
        absExponent = (unsigned int)(-exponent);
    
    double result = powerUnsigned(abs, absExponent);
    if(exponent<0)
        return 1.0/result;
    return result;
}

double powerUnsigned(double base, int exponent)
{
    if(exponent == 0)
        return 1.0;
    if(exponent == 1)
        return base;
    double result = powerUnsigned(base, exponent>>1);
    result *= result;
    if(exponent&1 == 1)
        result *= base;
    return result;    
}
```



