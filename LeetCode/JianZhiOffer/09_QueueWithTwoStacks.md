# 用两个栈模拟一个队列

### Solution

```c++
template <typename T> class CQueue
{
public:
    CQueue(void);
    ~CQueue(void);
    
    void appendTail(const T& node);

    T deleteHead();

private:
    stack<T> stack1;
    stack<T> stack2;
};

template<typename T> void CQueue<T>::appendTail(const T& element)
{
    stack1.push(element);
} 

template<typename T> T CQueue<T>::deleteHead()
{
    if(stack2.size()<= 0)
    {
        while(stack1.size()>0)
        {
            T& data = stack1.top();
            stack1.pop();
            stack2.push(data);
        }
    }

    if(stack2.size() == 0)
        throw new exception("queue is empty");

    T head = stack2.top();
    stack2.pop();

    return head;
}

```



# 用两个队列模拟一个栈



```c++
template<typename T> class CStack
{
public:
	CStack(void);
	~CStack(void);
 
	void appendTail(const T& node);
	T deleteHead();
 
private:
	queue<T> q1;
	queue<T> q2;
};
 
template<typename T>
void CStack<T>::appendTail(const T& node)//实现栈元素的插入
{
	//数据的插入原则：保持一个队列为空，一个队列不为空，往不为空的队列中插入元素
	if (!q1.empty())
	{
		q1.push(node);
	}
	else
	{
		q2.push(node);
	}
}
 
template<typename T>
T CStack<T>::deleteHead()//实现栈元素的删除
{
	int ret = 0;
	if (!q1.empty())
	{
		int num = q1.size();
		while (num > 1)
		{
			q2.push(q1.front());
			q1.pop();
			--num;
		}
		ret = q1.front();
		q1.pop();
	}
	else
	{
		int num = q2.size();
		while (num > 1)
		{
			q1.push(q2.front());
			q2.pop();
			--num;
		}
		ret = q2.front();
		q2.pop();
	}
	return ret;
}

```

