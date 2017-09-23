---
title: 大数模板
date: 2016-10-01
tags:
- Templates
toc: true
---

```
#include <cstdio>
#include <cstring>
#include <cstdlib>

#define MAXL 4
#define M10 1000000000
#define Z10 9

const int zero[MAXL - 1] = {0};
struct bnum
{
    int data[MAXL]; //  断成每截9个长度

    //  读取字符串并转存
    void read()
    {
        memset(data, 0, sizeof(data));
        char buf[32];
        scanf("%s", buf);
        int len = (int)strlen(buf);
        int i = 0, k;
        while (len >= Z10)
        {
            for (k = len - Z10; k < len; ++k)
            {
                data[i] = data[i] * 10 + buf[k] - '0';
            }
            ++i;
            len -= Z10;
        }
        if (len > 0)
        {
            for (k = 0; k < len; ++k)
            {
                data[i] = data[i] * 10 + buf[k] - '0';
            }
        }
    }

    bool operator == (const bnum &x)
    {
        return memcmp(data, x.data, sizeof(data)) == 0;
    }

    bnum & operator = (const int x)
    {
        memset(data, 0, sizeof(data));
        data[0] = x;
        return *this;
    }

    bnum operator + (const bnum &x)
    {
        int i, carry = 0;
        bnum ans;
        for (i = 0; i < MAXL; ++i)
        {
            ans.data[i] = data[i] + x.data[i] + carry;
            carry = ans.data[i] / M10;
            ans.data[i] %= M10;
        }
        return  ans;
    }

    bnum operator - (const bnum &x)
    {
        int i, carry = 0;
        bnum ans;
        for (i = 0; i < MAXL; ++i)
        {
            ans.data[i] = data[i] - x.data[i] - carry;
            if (ans.data[i] < 0)
            {
                ans.data[i] += M10;
                carry = 1;
            }
            else
            {
                carry = 0;
            }
        }
        return ans;
    }

    //  assume *this < x * 2
    bnum operator % (const bnum &x)
    {
        int i;
        for (i = MAXL - 1; i >= 0; --i)
        {
            if (data[i] < x.data[i])
            {
                return *this;
            }
            else if (data[i] > x.data[i])
            {
                break;
            }
        }
        return ((*this) - x);
    }

    bnum & div2()
    {
        int  i, carry = 0, tmp;
        for (i = MAXL - 1; i >= 0; --i)
        {
            tmp = data[i] & 1;
            data[i] = (data[i] + carry) >> 1;
            carry = tmp * M10;
        }
        return *this;
    }

    bool is_odd()
    {
        return (data[0] & 1) == 1;
    }

    bool is_zero()
    {
        for (int i = 0; i < MAXL; ++i)
        {
            if (data[i])
            {
                return false;
            }
        }
        return true;
    }
}

```