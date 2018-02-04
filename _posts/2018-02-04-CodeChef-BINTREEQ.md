---
layout: post
title:  "CodeChef BINTREEQ - Queries on a Binary Tree"

time: 02:00
categories: CodeChef
---

**Problem:** [Queries on a Binary Tree](https://www.codechef.com/problems/BINTREEQ)

*Cross-posted on [UIUC ICPC website](http://icpc.cs.illinois.edu/)*

**Solution:**
Let \\((w, t)\\) be a pair which has the same path configuration as 
\\((u, v)\\). Let \\(p = LCA(w, t)\\) be the lowest common ancestor of 
\\((w, t)\\). The insight here is that we only need to find the largest \\(p\\) 
possible under given constraints.

Assume \\(v > u\\), then we can simply start from \\(n\\) and follow the same 
path as \\(v\\) on its way back to \\(LCA(u, v)\\). Note such path from
\\(n\\) is not always possible, and we need to make minor adjustments during 
the trace.

let \\(n'\\) be the current node tracing from \\(n\\),
\\(v'\\) be the current node tracing from \\(v\\). If
\\(n'\\) cannot follow \\(v'\\) in the next step, then \\(n' - 1\\) must able 
to follow (Hint: parity determines whether \\(n'\\) is able to follow or not).

**Complexity:** O(\\(Q\log n\\))


**Source Code:**
{% highlight C++ %}
{% raw %}
#include <iostream>
#include <cmath>
#include <algorithm>

using namespace std;

int main() {
    long long Q, u, v, n;
    scanf("%lld", &Q);
    while (Q--) {
        scanf("%lld %lld %lld", &n, &u, &v);
        if (u > v)
            swap(u, v);

        while (u != v) {
            if (v > u) {
                if (v % 2 == 1) {
                    if (n % 2 == 0)
                        n -= 1;
                }
                else {
                    if (n % 2 == 1)
                        n -= 1;
                }
                v >>= 1;
                n >>= 1;
            }
            else {
                u >>= 1;
            }
        }
        printf("%lld\n", n);
    }
    return 0;
}
{% endraw %}
{% endhighlight %}
