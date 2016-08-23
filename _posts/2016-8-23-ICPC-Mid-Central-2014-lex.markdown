---
layout: post
title:  "ICPC Mid-Central 2014 - Lexicography"
date:   2016-08-23 17:00:00 -0600
categories: ICPC
---

**Problem:** [Lexicography]

**Solution:**
Given the huge upper bound of K, it is not possible to brute force the answer. Instead, while solving the problem,
we can skip a lot of intermediate strings to improve efficiency.
First, we should sort all the available characters alphabetically. Then, for every character in the list,
we try to fix it at a certain position, and calculate the number of strings possible with the rest of the characters. If the number is 
smaller than K, then we should skip all the strings starting with this character, and decrease K by that number. If the number of strings is
greater or equal to K, then it means the current character should indeed be at the current position. We add it to the output, and remove 
that character from the list.

**Source Code:**
{% highlight C++ tabsize=4 %}


#include <iostream>
#include <cstdio>
#include <string>
#include <algorithm>
#include <cmath>

using namespace std;

int main() {
    string word;
    long long K;

    long long fact[20] = {1};
    for (int i = 1; i < 20; i++)
        fact[i] = fact[i - 1] * i;

    while (cin >> word >> K && word != "#") {
        sort(word.begin(), word.end());
        
        int count[26] = {0};
        long long skip = fact[word.size() - 1];

        for (int i = 0; i < word.size(); i++) 
            count[word[i] - 'A']++;

        for (int i = 0; i < 26; i++)
            if (count[i])
                skip /= fact[count[i]];

        int digit = 0;
        
        string answer;
        while (digit < word.size()) {
            for (int i = 0; i < 26; i++)
                if (count[i]) {

                    count[i]--;

                    skip = fact[word.size() - 1 - digit];
                    for (int i = 0; i < 26; i++)
                        if (count[i])
                            skip /= fact[count[i]];

                    if (skip < K) {
                        K -= skip;
                        count[i]++;
                    }
                    else {
                        answer += ('A' + i);
                        break;
                    }
                }

            digit++;

        }
        cout << answer << "\n";
    }
} 
{% endhighlight %}

[Lexicography]: https://icpcarchive.ecs.baylor.edu/index.php?option=com_onlinejudge&Itemid=8&category=662&page=show_problem&problem=4826
