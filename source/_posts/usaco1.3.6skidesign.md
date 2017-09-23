---
title: USACO1.3.6skidesign
date: 2016-06-01
categories:
- 基础算法
- 贪心
tags:
- USACO
toc: true
---

## 题目
滑雪训练营的最高和最低的山峰海拔高度差大于17就要收税。因此,如果他改变山峰的高度（使最高与最低的山峰海拔高度差不超过17）,约翰可以避免支付税收。
如果改变一座山x单位的高度成本是x^2单位,约翰最少需要付多少钱?


## 分析
枚举所有长度为17的间隔并求对应费用，如1和18，即求把所有高度大于18的山变为18和小于1的山变为1的费用,然后对所有间隔对应的费用求最小值 


```
/*
ID:15829681
LANG:C++
TASK:skidesign
*/
#include <cstdio>
#include <algorithm>
#include <iostream>
#define LOCAL
#define N 1005
#define MAX 0x3f3f3f3f
using namespace std;
int a[N];
int n;
int dfs(int be,int en){
      int b[N],c[N];
      int i,j,k,num;
      j=k=0;
      num=0;
      //cout<<n<<endl;
      for(i=1;i<=n;i++){
            //cout<<be<<' '<<en<<' '<<a[i]<<endl;
            if(a[i]<be){
                  b[++j]=i;
                  //cout<<b[j];
            }
            if(a[i]>en){
                  c[++k]=i;
                  //cout<<c[k];
            }
      }
      for(i=1;i<=j;i++){
            //cout<<b[i];
            num+=(be-a[b[i]])*(be-a[b[i]]);
      }
      for(i=1;i<=k;i++){
            //cout<<c[i];
            num+=(a[c[i]]-en)*(a[c[i]]-en);
      }
      //cout<<num<<' ';
      return num;
}
bool cmp(int a,int b){
      return a<b;
}
int main()
{
      #ifdef LOCAL
      freopen("skidesign.in","r",stdin);
      freopen("skidesign.out","w",stdout);
      #endif // LOCAL
      int i,now,minm;//全局变量和局部变量重名。。。。。。。。。
      scanf("%d",&n);
      for(i=1;i<=n;i++){
            scanf("%d",&a[i]);
      }
      sort(a+1,a+1+n,cmp);
      minm=MAX;
      for(i=0;i<=93;i++){
            now=dfs(i,i+17);
            if(minm>now){
                  minm=now;
                  //cout<<minm<<' ';
            }
      }
      printf("%d\n",minm);
}

```
