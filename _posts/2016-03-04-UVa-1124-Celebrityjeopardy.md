---
layout: post
title:  "UVa 1124 - Celebrity jeopardy"
date:   2016-03-4 20:55:00 -0600
categories: UVa
---

**Problem:** [Celebrity jeopardy]

**Solution:**
Just echo the input.
Triage is important in competitions.

**Source Code:**
{% highlight C++ %}
#include <string>
#include <iostream>

using namespace std;

int main(){
    string line;
    while (getline(cin, line) && !line.empty())
        cout << line << "\n";
}


{% endhighlight %}

[Celebrity jeopardy]: https://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=3565
