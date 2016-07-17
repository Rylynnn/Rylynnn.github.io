---
title: USACO1.1.4beads
date: 2016-04-21 
categories:
- USACO
tags:
- USACO
- 乱搞
- 基本DP
toc: true
---

## 题目

一串珠子有红的，蓝的，白的，找个地方剪开，从左边或者右边开始收集颜色相同的珠子，直到遇到不同颜色的珠子停止收集。


## 分析

首先肯定是变环为链。

乱搞：
    我已经看不懂我以前写的乱搞了。。。生无可恋.jpg
    看完发现是个纯 纯 纯模拟。。。


```
/*
ID: 15829681
LANG: C++
TASK: beads
*/
#include <cstdio>
#include <string>
#include <iostream>
#define LOCAL
using namespace std;
int main()
{
    #ifdef LOCAL
    freopen("beads.in","r",stdin);
    freopen("beads.out","w",stdout);
    #endif
    int n;
    string a,b;
    scanf("%d",&n);
    cin>>a;
    b=a+a;
    int maxm=0,i,j,num,p;
    char col;
    for(i=0;i<a.size();i++){//对原来的环中的每个点，在b（a断环变成的链）里做链子长度的模拟
        col=a[i];
        num=1,p=0;
        for(j=i+1;j<i+n;j++){//纯种的分情况讨论。。。
            if(b[j]!='w'){
                if(col=='w'){
                    col=b[j];
                    num++;
                }
                else{
                    if(col!=b[j]){
                        if(p==0){
                            col=b[j];//忘了第一次转变之后前驱要更换。。。
                            p=1;
                            num++;
                        }
                        else{
                            break;
                        }
                    }
                    else{
                        num++;
                    }
                }
            }
            else{
                num++;
            }
            if(num>a.size()){
                num=a.size();
                break;
            }
        }
        if(num>maxm){
            maxm=num;
        }
    }
    printf("%d\n",maxm);
}

```

DP：

我们只要求出bl,br,rl,rr，那么结果就是max(max(bl[i],rl[i])+max(br[i+1],rr[i+1])) (0<=i<=2*n-1)。
我们以求bl,rl为例:
初始时bl[0]=rl[0]=0
我们从左往右计算
如果necklace[i]='r',rl[i]=rl[i-1]+1,bl[i]=0；
如果necklace[i]='b', bl[i]=bl[i-1]+1,rl[i]=0；
如果necklace[i]='w',bl[i]=bl[i-1]+1,rl[i]=rl[i-1]+1。


```
/*
ID: 15829681
LANG: C++
TASK: beads
*/
/*
切割点左右max(br[i],rr[i])+max(bl[i+1],rl[i+1])
*/
#include <cstdio>
#include <cstring>
#include <iostream>
#include <string>
#define N 800
#define CLE(a) memset(a,0,sizeof(0))
using namespace std;
int n,br[N],bl[N],rr[N],rl[N];
int main()
{
    freopen("beads.in","r",stdin);
    freopen("beads.out","w",stdout);
    string a,s;
    int ans;
    while(scanf("%d",&n)!=EOF){
        cin>>a;
        CLE(br);
        CLE(bl);
        CLE(rr);
        CLE(rl);
        ans=-1;
        s=a+a;
        s=" "+s;
        br[0]=rr[0]=0;
        for(int i=1;i<s.size();i++){
            if(s[i]=='r'){
                rr[i]=rr[i-1]+1;
                br[i]=0;
            }
            else if(s[i]=='b'){
                br[i]=br[i-1]+1;
                rr[i]=0;
            }
            else if(s[i]=='w'){
                br[i]=br[i-1]+1;
                rr[i]=rr[i-1]+1;
            }
            if(br[i]>n){
                br[i]=n;
            }
            if(rr[i]>n){
                rr[i]=n;
            }
        }
        bl[s.size()]=rl[s.size()]=0;
        for(int i=s.size()-1;i>0;i--){
            if(s[i]=='r'){
                rl[i]=rl[i+1]+1;
                bl[i]=0;
            }
            else if(s[i]=='b'){
                bl[i]=bl[i+1]+1;
                rl[i]=0;
            }
            else if(s[i]=='w'){
                bl[i]=bl[i+1]+1;
                rl[i]=rl[i+1]+1;
            }
            if(bl[i]>n){
                bl[i]=n;
            }
            if(rl[i]>n){
                rl[i]=n;
            }
        }
        // num=0;
        for(int i=1;i<s.size();i++){
            ans=max(ans,max(br[i],rr[i])+max(bl[i+1],rl[i+1]));
            //num=i;
        }
        //cout<<num;
        printf("%d\n",min(ans,n));
    }

    return 0;
}

```
