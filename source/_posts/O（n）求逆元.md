---
title: 逆元和欧拉函数
date: 2016-09-06 
categories:
- 数学
- 数论
- 逆元和欧拉函数
tags:
- 学习笔记
- Templates
toc: true
---

## 定义

对于正整数a和m，如果有ax≡1(mod m)，那么把这个同余方程中的最小正整数解叫做a模m的逆元。

## 前提

求$a(mod m)$意义下的逆元,要求a与m互质,否则不存在乘法逆元


## 欧拉定理

### 欧拉函数

#### O(√n)求单个数的欧拉函数

对正整数n，欧拉函数是少于或等于n的数中与n互质的数的数目。例如euler(8)=4，因为1,3,5,7均和8互质。
Euler函数表达通式：
$euler(x)=x(1-{\frac{1}{p_1}})(1-{\frac{1}{p_2}})(1-{\frac{1}{p_3}})(1-{\frac{1}{p_4}})…(1-{\frac{1}{p_n}})$
其中p_1,p_2……p_n为x的所有素因数，x是不为0的整数。$euler(1)=1$（唯一和1互质的数就是1本身）。 

**欧拉公式的延伸：一个数的所有质因子之和是$euler(n) * n/2$。**
```
#include<cstdio>
#include<cstring>
#include<algorithm>
#define MAXN 200010
#include<iostream>
#include<cmath>
using namespace std;
int main()
{
    int n,i,temp;
    while(scanf("%d",&n)!=EOF){
        temp=n;
        for(i=2;i*i<=n;i++){
          if(n%i==0){
              while(n%i==0) n=n/i;
              temp=temp/i*(i-1);
          }
          if(n<i+1)
              break;
        }
        if(n>1)
            temp=temp/n*(n-1);
        printf("%d\n",temp);
    }
    return 0;
}
```

#### O(n)求1~n的素数和欧拉函数

```
int prime[N],phi[N],cnt;// prime:记录质数，phi记录欧拉函数  
int Min_factor[N];// i的最小素因子  
bool vis[N];  
void Init()  
{  
    cnt=0;  
    phi[1]=1;  
    int x;  
    for(int i=2;i<N;i++)  {  
        if(!vis[i])  {  
            prime[++cnt]=i;  
            phi[i]=i-1;  
            Min_factor[i]=i;  
        }  
        for(int k=1;k<=cnt&&prime[k]*i<N;k++)  {  
            x=prime[k]*i;  
            vis[x]=true;  
            Min_factor[x]=prime[k];  
            if(i%prime[k]==0){  
                phi[x]=phi[i]*prime[k];  
                break;  
            }  
            else phi[x]=phi[i]*(prime[k]-1);  
        }  
    }  
}  
```

### 欧拉函数求逆元
根据欧拉定理
                $$a^{ϕ(m)}≡1(mod m)$$
                $$a^{(ϕ(m)−1)}*a≡1(mod m)$$
                $$a^{(ϕ(m)−1)}≡a^-1(mod m)$$                       
 
求现在来看一个逆元最常见问题，求如下表达式的值（已知）
                $$ans=(a/b)(mod m)$$
 
当然这个经典的问题有很多方法，最常见的就是扩展欧几里得，如果是素数，还可以用费马小定理。
但是你会发现费马小定理和扩展欧几里得算法求逆元是有局限性的，它们都会要求与互素。实际上我们还有一
种通用的求逆元方法，适合所有情况。公式如下
                $$ans=(a/b)(mod m)=(a mod mb)/b$$         

## Extended_GCD

[GCD和Extended_GCD（多元不定方程）](http://rylynnn.github.io./2016/04/30/GCD%E5%92%8CExtended_GCD/)

## O(N)求$1~n < p$的所有逆元

新学的一个求逆元的方法：
    $$inv[i]=(MOD-MOD/i) * inv[MOD\mod i]\mod MOD$$
证明：
设
$$t = MOD / i$$
$$k = MOD\mod i$$
则有
$$t * i + k = 0\mod MOD$$
有
$$-t * i = k\mod MOD$$
两边同时除以ik得到
$$-t * inv[k] = inv[i]\mod MOD$$
即
$$inv[i] = -MOD / i * inv[MOD\mod i]$$
即
$$inv[i] = ( MOD - MOD / i) * inv[MOD\mod i]$$
证毕
适用于MOD是质数的情况，能够O(n)时间求出1~n对模MOD的逆

```
#include<cstdio>
#include<cstring>
#include<algorithm>
#define MAXN 200010
#include<iostream>
#include<cmath>
using namespace std;
int main()
{
    int n,mod;
    int x[MAXN];
    while(scanf("%d%d",&n,&mod)!=EOF){
        x[1]=1;
        for(int i=2;i<=n;i++){//在最后求值的时候，对1~n的每个数先取模，再求其x[i%m]的值
            x[i]=((mod-mod/i)*x[mod%i])%mod;
        }
        for(int i=1;i<=n;i++){
            printf("%d ",x[i%mod]);
        }
    }
    return 0;
}
```

## O(n)求n!的逆元

预处理出来n的阶乘存在fac[n]里面，然后通过计算n!的逆元，倒着求出每个i!(i < n)的逆元。

```
#include<cstdio>
#include<cstring>
#include<algorithm>
#define MAXN 200010
#include<iostream>
#include<cmath>
using namespace std;
int main()
{
    int n,mod;
    int x[MAXN];    
    fac[0]=fac[1]=1;
    for(int i=2;i<=MAXN;i++){
        fac[i]=fac[i-1]*i%mod;
    }
    inv[MAXN]=pow_mod(fac[MAXN],phi(mod)-1);//mod为质数时为phi(mod)=mod-1
    for(int i=MAXN-1;i>=0;i--){
        inv[i]=inv[i+1]*(i+1)%mod;
    }
    return 0;
}

```
