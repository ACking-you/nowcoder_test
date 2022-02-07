[toc]

## 题目

[题目链接](https://ac.nowcoder.com/acm/contest/26908/1026)

![](https://s2.loli.net/2022/01/15/BPuXYoDekUWxEtA.png)

## 题目详解

> 以下为手写详解

![](https://s2.loli.net/2022/01/15/A6VJRB3SlDtbcK7.jpg)

下面总结这个做题步骤：

1. 二分搜索可能的最大单位价值。
2. 根据这个值得到每个数的单位价值情况s，根据s的值排序，得到前k大的s，相加得出我们枚举的这个最大单位价值是大了还是小了，方便后续二分！

> 最后按保留两位小数，输出即可。

## 解题代码

```cpp
#include<bits/stdc++.h>
using namespace std;
const int maxn = 1e5+5; 
int w[maxn],v[maxn];
double s[maxn]; //TODO 存下用于比对实际单位价值的信息
int n,k;
using namespace std;

int solve(double mid)
{
	double ss = 0;
	for(int i = 0;i<n;i++)
	{
		s[i] = 1.0*v[i] - w[i]*mid;
	}
	sort(s,s+n,greater<double>());//TODO 从大到小排序，因为要取前k个最大值
	for(int i = 0;i<k;i++)
		ss += s[i];
    //TODO 得到前k个最大的s信息的和，
    // 如果大于0，则说明取小了，小于0则取大了，等于0则正好满足！
	if(ss>0) return 1;
    else if(ss==0)return -1;
	else return 0;
}
int main()
{
	int t;
	cin>>t;
	while(t--)
	{
		scanf("%d %d",&n,&k);
		double low = 0,high = 100000,mid;
		for(int i = 0;i<n;i++)
		{
			scanf("%d %d",&w[i],&v[i]);
		}
        //TODO 二分搜索，由于double类型的二分，故取一个小数形式进行比对
        // 注意double类型的二分，千万别+1或者-1！这个跨度太大了，因为我们搜的是个分数！
		while(high - low >0.00001)
		{
			mid = (low + high)/2.0;
            int flag = solve(mid);
			if(flag==1) low = mid;//TODO 收缩左边界
            else if(flag == -1)break;//TODO 枚举得到答案
			else high = mid;//TODO 收缩右边界
 
		}
		printf("%.2lf\n",mid);
	}
	return 0;
}
```

