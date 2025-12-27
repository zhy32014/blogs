---
title: 12-27 leetcode 每日一题
date: 2025-12-27 20:12:23
tags: 日常学习
---
![题面](image.png)

# 题意
先附上题目链接: 
https://leetcode.cn/problems/meeting-rooms-iii/description/
意思就是说题目给出一些会议的开始和结束时间，并且给你n个会议室，对于每个会议都会按照时间的先后按从小到大的顺序占用会议室,我们需要找到所有会议结束之后使用次数最多的会议室的序号为多少。

# 题目思路
题目意思比较简单，直接依据题意进行模拟即可。
首先对会议时间进行排序，随后使用一个小根堆记录当前空闲的会议室的序号，如果说存在空闲会议室，则直接去堆顶的元素分配给当前的会议，并记录会议室的使用次数，如果不存在会议，则需要将会议进行延期，直到有新的会议室空闲。
因此，除了使用小根堆对会议室进行储存之外，还需要用一个小根堆对正在进行的会议进行储存，存会议的结束时间和占用的会议室的序号。每次决定会议时，先将使用中的会议中在当前时间之前的会议都弹出到存空闲会议室的小根堆中，随后再进行会议打打分配。如果说此时不存在空闲会议室，那么则取出最早结束的会议室，并对时间进行更新，然后对当前的会议室进行分配。
感觉说的不是很清晰，所以直接上代码。

```cpp
#include<bits/stdc++.h>
using namespace std;

class Solution {
public:
    int mostBooked(int n, vector<vector<int>>& meetings) {
        sort(meetings.begin(),meetings.end());
        priority_queue<int,vector<int>,greater<int>> q;
        for(int i=0;i<n;i++) q.push(i);
        priority_queue<pair<long long,long long>,
        vector<pair<long long,long long>>,
        greater<pair<long long,long long>>> busy;
        vector<int> count(n+10,0);
        long long cur=0;
        for(auto i:meetings)
        {
            int st=i[0];
            int end=i[1];
            cur=max(cur,(long long)st);

            while(busy.size()&&busy.top().first<=cur)
            {
                int room=busy.top().second;
                busy.pop();
                q.push(room);
            }

            if(q.empty())
            {
                auto [end,room]=busy.top();
                busy.pop();
                cur=end;
                q.push(room);

                while(busy.size()&&busy.top().first<=cur)
                {
                    int room=busy.top().second;
                    busy.pop();
                    q.push(room);
                }
            }


            int room=q.top();
            q.pop();
            count[room]++;

            long long dur=end-st;
            busy.push({cur+dur,room});
        }
        int maxx=0;
        int id=0;
        for(int i=0;i<n;i++)
        {
            if(count[i]>maxx)
            {
                maxx=count[i];
                id=i;
            }
        }
        return id;

    }
};