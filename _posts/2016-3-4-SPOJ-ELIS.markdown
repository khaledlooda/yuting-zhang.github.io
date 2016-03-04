---
layout: post
title:  "SPOJ ELIS - Easy Longest Increasing Subsequence"
date:   2016-03-4 00:40:00 -0600
categories: UVa
---

**Problem:** [Easy Longest Increasing Subsequence]

**Solution:**
A very easy and straightforward DP refresher.
Given the extreme small input size, there is no need to use tail table.

**Souce Code:**
{% highlight C++ %}
#include <iostream>
#include <cstdio>
#include <vector>

using namespace std;

int main(){
    int N;
    scanf("%d", &N);
    vector<int> vec(N + 1);
    vec[0] = -1;
    for (int i = 0; i < N; i++)
        scanf("%d", &vec[i + 1]);
    vector<int> dp(N + 1);
    dp[0] = 0;
    for (int i = 1; i <= N; i++)
        for (int j = 0; j < i; j++)
            if (vec[j] < vec[i])
                if (dp[j] + 1 >= dp[i])
                    dp[i] = dp[j] + 1;
    int answer = 0;
    for (int i = 0; i <= N; i++)
        if (dp[i] > answer)
            answer = dp[i];
    printf("%d\n", answer);
}

{% endhighlight %}

[Easy Longest Increasing Subsequence]: http://www.spoj.com/problems/ELIS
