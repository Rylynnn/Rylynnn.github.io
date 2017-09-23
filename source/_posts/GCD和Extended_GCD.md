---
title: GCD和Extended_GCD（多元不定方程）
date: 2016-04-30 
categories:
- 数学
- 数论
- GCD和Extended_GCD
tags:
- 学习笔记
toc: true
---

# GCD

欧几里德算法又称辗转相除法，用于计算两个整数a,b的最大公约数。
gcd(a,b) 表示 a,b的最大公约数，一般程序里也用同名函数来计算最大公约数

## 证明 

若a,b都不为0，且a=bq+r,0≤r< b，则

        (a,b)=(b,r)

设d=(a,b),c=(b,r)
∵ a=bq+r,d|a,d|b
∴ d|r，即d≤c
∵ a=bq+r,c|b,c|r
∴ c|a,即c≤d
∴ d≤c且c≤d，即c=d，(a,b)=(b,r)

# Extended_GCD

* 求解二元一次不定方程的解

对任意二元一次不定方程ax+by=n:
* 当n%gcd(a,b)==0时有整数解
    当gcd(a,b)==1时，总能通过gcd倒推(exgcd)，找到一组整数解x0,y0；
    当gcd(a,b)!=1时，令n0=n/gcd(a,b)，b0=b/gcd(a,b),a0=a/gcd(a,b)，则通过gcd倒推(exgcd)，找到一组整数解x0,y0，则n0*x0,n0*y0为原二元一次不定方程的一组整数解；
    通解：

       x=n0*x0+b0*t
       y=n0*y0–a0*t    (t=0,1,2,…)

* 当n%gcd(a,b)!=0时无整数解

| GCD           | EX_GCD                                  | 
| ------------- |:---------------------------------------:| 
| a=q1*b+r1     | r1=a-q1*b                               | 
| b=q2*r1+r2    | r2=b-q2*r1                              | 
|               |   =b-q2*(a-q1*b)                        | 
|               |   =-q1*a+(1-q1*q2)*b                    | 
| r1=q3*r2+r3   | r3=r1-q3*r2                             | 
|               |   =(a-q1*b)-q3*(-q1*a+(1-q1*q2)*b)      | 
| ...           |   ...                                   | 

* 计算a%b及b%a的乘法逆元
如果gcd(a，b)=d，则存在m，n，使得: 
称呼这种关系为a、b组合整数d，m，n称为组合系数。当d=1时，有 ma + nb = 1 ，此时可以看出m是a模b的乘法逆元，n是b模a的乘法逆元。

* 求解模线性方程（线性同余方程）；