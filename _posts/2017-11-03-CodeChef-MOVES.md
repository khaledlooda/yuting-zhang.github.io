---
layout: post
title:  "CodeChef MOVES - Robot Movings"

time: 02:00
categories: CodeChef
---

**Problem:** [Robot Movings](https://www.codechef.com/problems/MOVES)

*Cross-posted on [UIUC ICPC website](http://icpc.cs.illinois.edu/)*

**Solution:**
This problem is in fact a combinatorics problem.
We try to find the number of ways we can make down turns, and also the 
number of ways to we can make right turns.

Consider \\(N, K\\). 

If \\(K\\) is odd, then we can either

* Make more down turns than right turns. To be specific, we can
make \\((K + 1) / 2\\) down turns and \\((K - 1) / 2\\) right turns.
However since the last down turn will always happen at the last column,
we can only choose the positions of \\((K - 1) / 2\\) down turns 

* Make more right turns than down turns. This case is actually symmetric to the
previous one.

If \\(K\\) is even, then we will always make the same number of 
right turns and down turns. However be careful  that the last turn is always 
going to happen at the last column or last row, so we cannot choose the position
for that turn.

Then the problem becomes to find the values of
\\(\binom{n}{k}\\) where \\(n, k \le 5000\\). Simply construct
Pascal's triangle level by level will have time complexity of O(\\(n^2\\)), 
which is acceptable for this problem given \\(n\\) is at most 5000.

In fact, we can also calculate \\(\binom{n}{k}\\) directly by using 
the formula \\(\binom{n}{k} = \frac{n!}{k!(n - k)!}\\). Since we are working
under mod \\(p = 1000000007\\), we cannot do the calculation naively, because
\\(n! \mod p\\) might not be divisible by \\(k!(n - k)! \mod p\\). Instead,
we find the inverses of \\(k!\\) and \\((n - k)!\\) under mod \\(p\\).

Since \\(p\\) is prime, by Fermat's Little Theorem, we have
\\(a^{p - 1} \equiv 1 \mod p\\) as long as \\(p\\) does not divide \\(a\\).
So we have \\((a)(a^{p - 2}) \equiv 1 \mod p\\). Then we have
\\(a^{p - 2}\\) to be the inverse of \\(a\\) under mod \\(p\\). 

Because we are working under mod \\(p\\) for all factorials, the value of 
factorial will always be smaller than \\(p\\), 
and so \\(p\\) does not divide the factorial.
Hence Fermat's Little Theorem is applicable and  we can use the 
aforementioned way to find the inverses of each factorial,
and finally apply the formula.

Note the sample solution uses this faster linear solution.

**Complexity:** O(\\(N + T\\)) where \\(T\\) is the number of test cases.

**Source Code:**
{% highlight C++ %}
{% raw %}
#include <cstdio>

#define MOD 1000000007
#define MAX 5010

int N, K;
long long fact[MAX];
long long inv_fact[MAX]; 

inline long long mul(long long a, long long b) {
    return a * b % MOD;
}

long long pow(long long n, long long p) {
    if (p == 0)
        return 1;
    long long res = pow(n, p >> 1);
    res = mul(res, res);
    if (p & 1) 
        res = mul(res, n);
    return res;
}

long long binom(long long n, long long k) {
    return mul(mul(fact[n], inv_fact[k]), inv_fact[n - k]);
}

void build() {
    fact[0] = inv_fact[0] = 1;
    for (int i = 1; i < MAX; i++) {
        fact[i] = mul(fact[i - 1], i);
        inv_fact[i] = pow(fact[i], MOD - 2);
    }
}

int main() {
    build();
    while (scanf("%d %d", &N, &K) == 2 && N && K) {
        long long res = 0;
        K++;
        int down = (K >> 1) - 1, right = K - (K >> 1) - 1;
        res = mul(binom(N - 2, down), binom(N - 2, right));
        res = mul(res, 2);
        printf("%lld\n", res);
    }
    return 0;
}
{% endraw %}
{% endhighlight %}
