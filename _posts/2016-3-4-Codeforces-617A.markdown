---
layout: post
title:  "Codeforces 617A Elephant"
date:   2016-03-4 20:00:00 -0600
categories: Codeforces
---

**Problem:** [Elephant]

**Solution:**
Starter's DP problem.

**Source Code:**
{% highlight C++ %}
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <vector>

using namespace std;

int main(){
	int x;
	scanf("%d", &x);
	vector<int> dp(x + 1, x * 2);
	dp[0] = 0;
	for (int i = 1; i <= x; i++)
		for (int j = 1; j <= 5; j++)
			if (i - j >= 0 && dp[i - j] + 1 < dp[i])
				dp[i] = dp[i - j] + 1;
	printf("%d\n", dp[x]);
}
{% endhighlight %}

[Elephant]: http://codeforces.com/problemset/problem/617/A
