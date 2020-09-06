小Q的父母要出差N天，走之前给小Q留下了M块巧克力。小Q决定每天吃的巧克力数量不少于前一天吃的一半，但是他又不想在父母回来之前的某一天没有巧克力吃，请问他第一天最多能吃多少块巧克力

### Solution

```c++
#include<iostream>
using namespace std;
int n,m;
//计算第一天吃s个巧克力一共需要多少个多少个巧克力
int sum(int s){
    int sum=0;
    for(int i=0;i<n;i++){
        sum+=s;
        s=(s+1)>>1;//向上取整
    }
    return sum;
}
//二分查找
int fun(){
    if(n==1) return m;
    int low=1;
    int high=m;//第一天的巧克力一定是大于等于1小于等于m的
    while(low<high){
        int mid=(low+high+1)>>1;//向上取整
        if(sum(mid)==m) return mid;//如果第一天吃mid个巧克力，刚刚好吃完所有巧克力，那么直接返回
        else if(sum(mid)<m){
            low=mid;
        }else{
            high=mid-1;
        }
    }
    return high;
}
int main()
{
    cin>>n>>m;
    int res=fun();
    cout<<res<<endl;
    return 0;
}
```

