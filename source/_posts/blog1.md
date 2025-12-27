---
title: 每日一题
date: 2025-12-26 16:14:29
tags: 学习
---
# 题面
![题面](image.png)
![alt text](image1.png)

# 解析
简单来说此题就是要寻找一个位置，在这个位置之前N会产生贡献，在这个位置之后Y会产生贡献，因此直接考虑前缀和后缀的和，前缀计算N给出的贡献，后缀计算Y给出的贡献，最后遍历所有位置计算贡献最小值即可。
比较水的一道题
代码如下

```cpp
#include<bits/stdc++.h>
using namespace std;
class Solution {
public:
    int bestClosingTime(string customers) {
        string s = customers;
        int n = s.size();
        s = " " + s;
        vector<int> pre(n+10, 0);
        vector<int> suf(n+10, 0);
        for(int i=1; i<=n; i++){
            pre[i] = pre[i-1];
            if(s[i] == 'N') pre[i]++;
        }
        for(int i=n; i>=0; i--){
            suf[i] = suf[i+1];
            if(s[i] == 'Y') suf[i]++;
        }
        int ans = 10000000000;
        int pos = 0;
        for(int i=0; i<=n; i++){
            int res = pre[i] + suf[i+1];
            if(res < ans){
                ans = res;
                pos = i;
            }
        }
        return pos;
    }
};