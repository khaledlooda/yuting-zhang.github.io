---
layout: post
title:  "Codeforces 257C - View Angle"
date:   2016-9-15 9:30:00 -0500
categories: ICPC
---

**Problem:** [View Angle]

**Solution:**
In general, the goal of this problem is to find the maximum angle between 
two adjacent mannequins, which is the angle we will 
include into our view range. 

The cmath library will come in handy in this case.
With atan2 function, we can easily calculate the
angle between a mannequin and the positive x-axis. Also, with acos(-1), we can have a 
relatively accurate \\(\pi\\) value.



**Source Code:**
{% highlight C++ %}
#include <cmath>
#include <iostream>
#include <map>
#include <utility>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
	vector<double> angles;

	int n;
	cin >> n;
	for (int i = 0; i < n; i++) {
		int x, y;
		cin >> x >> y;
		double angle = atan2(y, x);
		angle = angle * 180 / acos(-1);
		if (angle < 0)
			angle += 360;
		angles.push_back(angle);
	}

	double maxAngle = 0;
	sort(angles.begin(), angles.end());

	for (int i = 0; i < n - 1; i++)
		maxAngle = max(maxAngle, angles[i + 1] - angles[i]);

	maxAngle = max(maxAngle, angles.front() + 360 - angles.back());

	printf("%.15f\n", 360 - maxAngle);
}
{% endhighlight %}

[View Angle]: http://codeforces.com/problemset/problem/257/C
