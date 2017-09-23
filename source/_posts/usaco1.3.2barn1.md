---
title: USACO1.3.2barn1
date: 2016-04-21 
categories:
- 动态规划
- 基础DP
tags:
- USACO
toc: true
---

## 题目

有m个牛棚，有n个牛，住在不同的牛棚（不住满），要用k个木板封住所有有牛的牛棚，问木板的最小长度


## 分析 

贪心：
显然，当所有木板均被用上时，长度最短（数量最多）（证明....）。 正向思维，初始状态有c块木板，每块木板只盖住一个牛棚。
由c块倒推至m块木板，每次寻找这样的两个牛棚：其间距在所有未连接的木板中最小。 当这两个牛棚的木板连接时，总木板数减1，总长度增加最小

```
/*
ID: 15929681
LANG: C++
TASK: barn1
*/
#include <cstdio>
#include <cmath>
#include <iostream>
#include <algorithm>
#define N 205
#define LOCAL
using namespace std;
bool cmp(int a,int b){
      return a>b;
}
bool cm(int a,int b){
      return a<b;
}
int main()
{
      #ifdef LOCAL
            freopen("barn1.in","r",stdin);
            freopen("barn1.out","w",stdout);
      #endif
      int m,s,c,i,b;
      int a[N],dis[N];
      scanf("%d%d%d",&m,&s,&c);
      b=0;
      for(i=1;i<=c;i++){
            scanf("%d",&a[i]);
      }
      if(m>=c){//如果木板数足够多，则每个牛棚一个木板就可以了。
            printf("%d\n",c);
            return 0;
      }
      sort(a+1,a+1+c,cm);
      for(i=1;i<c;i++){
            //printf("%d",a[i]);
            dis[i]=a[i+1]-a[i]-1;//先是每两个牛之间就放一个木板，然后从距离最大的木板开始拆，直到剩m个木板
            //cout<<dis[i]<<" ";
      }
      sort(dis+1,dis+c,cmp);
      b=fabs(a[1]-a[c])+1;
      for(i=1;i<=m-1;i++){
            b-=dis[i];
      }
      printf("%d\n",b);
      return 0;
}

```

DP：
将所有牛的牛棚序号a[]排序后,设

      dp[i][j]表示用前i个木板修到第j头牛所用的最短长度,初始化为第一个木板修到第i头牛需要花的长度为牛棚到第一个牛棚的长度+1.

每次在两个决策中选择：
1.在当前这个牛棚处，新接一块新的木板。
2.将上一个牛棚的木板长度加长至可以掩盖当前牛棚。

```
/*
ID: 15929681
LANG: C++
TASK: barn1
*/
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#define MAX 0x3f3f3f3f
#define LOCAL
using namespace std;
int main()
{
    #ifdef LOCAL
        freopen("barn1.in","r",stdin);
        freopen("barn1.out","w",stdout);
    #endif
    int m,s,c,ans,minm;
    int a[507],dp[507][507];
    while(scanf("%d%d%d",&m,&s,&c)!=EOF){
        memset(dp,0,sizeof(dp));
        for(int i=1;i<=c;i++){
            scanf("%d",&a[i]);
        }
        sort(a+1,a+1+c);
        for(int i=1;i<=c;i++){
            dp[1][i]=a[i]-a[1]+1;
        }
        for(int i=2;i<=m;i++){
            for(int j=1;j<=c;j++){
                minm=MAX;
                for(int k=1;k<j;k++){
                    minm=min(dp[i][k]+a[j]-a[k],minm);
                }
                dp[i][j]=min(dp[i-1][j-1]+1,minm);
                //cout<<dp[i][j]<<' ';
            }
        }
        ans=0;
        printf("%d\n",dp[m][c]);
    }
    return 0;
}

```
