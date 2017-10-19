---
layout: post
title:  "Hackerrank lego-blocks - Lego Blocks"

time: 23:30
categories: Hackerrank
---

**Problem:** [Lego Blocks](https://www.hackerrank.com/contests/codesprint-practice/challenges/lego-blocks)

*Cross posted on [UIUC ICPC website](http://icpc.cs.illinois.edu/)*

**Solution:**
A typical DP problem. Notice that all blocks are of height 1, and only differ
in widths.

First let's consider \\(all\\_walls(h, w)\\), which gives the total number of
ways to build a wall of height \\(h\\) and  width \\(w\\), without considering
the requirement of solidness. Since we are not considering solidness, it is
clear that all rows are independent of each other. Let \\(rows(w)\\) be the
total number of ways to build a wall of height 1 and width \\(w\\), then we have
\\(all\\_walls(h, w) = rows(w)^h \\). This exponentiation can either be 
computed directly (O(\\(h\\)))  or use exponentiation by squaring
(O(\\(\log(h)\\))). Either way will work for this problem given height is at most
\\(1000\\).

To compute \\(rows(w)\\), we will use dynamic programming. The recurrence is 
simply 
\\[rows(w) = rows(w - 1) + rows(w - 2) + rows(w - 3) + rows(w - 4) \\]
We intentionally left out the base cases so readers can think by themselves.
If you are unsure, check the sample solution code at the end.

After discussing \\(all\\_walls(h, w)\\), now let's consider 
\\(solid\\_walls(h, w)\\), which is the total number of ways to build a
solid wall of height \\(h\\) and width \\(w\\). Directly counting the number
of solid walls efficiently is very hard. Instead, we can easily compute the
number of walls that are not solid given height and width.

If a wall is not solid, then by definition, we can find a vertical cut so that
no block is blocking that cut. In other words, this vertical cut effectively
partitions the original wall into two smaller walls. 

By enumerating the vertical cuts, we can split the original wall into two
smaller walls, a sign for another dynamic programming. However, in order to
avoid double counting, we should only enumerate the leftmost vertical cut.
Namely, the wall left of the vertical cut must be solid, and the wall right
of the vertical cut can either be solid or not. 

Let \\(vert\\_cut\\) be the position such that the leftmost vertical cut is 
on the right side of this position.
So we can compute \\(invalid\\_walls(height, width)\\) as 

\\[\sum_{vert\\_cut = 1}^{width - 1}
    solid\\_walls(height, vert\\_cut)
    \times all\\_walls(height, width - vert\\_cut)\\]

At last, we can get 
\\[solid\\_walls(height, width) = all\\_walls(height, width) - 
    invalid\\_walls(height, width) \\]

Filling up \\(all\\_walls\\) takes O(\\(NM\\)), and for each height, computing
\\(solid\\_walls\\) takes O(\\(M^2\\)). Since there are total \\(T\\) test case,
the time complexity will be O(\\(NM + TM^2\\)).

**Source Code:**
{% highlight C++ %}
{% raw %}
#include <cmath>
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

#define MOD 1000000007

// All possible ways to build a single row of wall.
long long row[1005] = {0};

// All possible ways to build a wall, including ones that are not solid.
long long all_walls[1005][1005]; 

// Ways to build solid walls.
long long solid_walls[1005][1005];

void populate_rows() {
    row[0] = 1;
    for (int i = 1; i <= 1000; i++) 
        for (int j = 1; j <= 4; j++)
            if (i - j >= 0)
                row[i] = (row[i - j] + row[i]) % MOD;
}

long long get_all_walls(int height, int width) {
    if (all_walls[height][width] != -1) 
        return all_walls[height][width];

    // base case
    if (height == 0)
        return all_walls[height][width] = 1;

    if (height % 2 == 0) {
        long long result = get_all_walls(height / 2, width);
        return all_walls[height][width] = result * result % MOD;
    } else {
        long long result = get_all_walls(height / 2, width);
        return all_walls[height][width] = 
            (result * result % MOD) * row[width] % MOD;
    }
}

long long solve(int height, int width) {
    if (solid_walls[height][width] != -1) // Have calculated before
        return solid_walls[height][width];

    long long invalid_walls = 0;
    if (width <= 1)
        invalid_walls = 0; // Base case. Only use 1*1*1 blocks.
    
    // Enumerate vertical cuts 
    for (int vert_cut = width - 1; vert_cut >= 1; vert_cut--) 
        invalid_walls = 
            (solve(height, vert_cut) * 
            get_all_walls(height, width - vert_cut) % MOD + 
            invalid_walls) % MOD;

    return solid_walls[height][width] = 
        (get_all_walls(height, width) - invalid_walls + MOD) % MOD;
}

int main() {
    int T, N, M;
    scanf("%d", &T);
    
    populate_rows();

    // Set all values to -1.
    memset(solid_walls, -1, sizeof solid_walls); 
    memset(all_walls, -1, sizeof all_walls); 
    
    for (int test_case = 0; test_case < T; test_case++) {
        scanf("%d %d", &N, &M);
        printf("%lld\n", solve(N, M));
    }
    return 0;
}
{% endraw %}
{% endhighlight %}
