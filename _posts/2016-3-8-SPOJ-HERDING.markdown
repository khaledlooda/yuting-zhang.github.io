---
layout: post
title:  "SPOJ HERDING - Herding"
date:   2016-03-08 20:50 -0600
categories: SPOJ
---

**Problem:** [Herding]

**Solution:**
A very easy graph traversal problem. Although it seems at first that this is a Strongly Connected Component problem, it is actually even much easier.
Since there is exactly one in edge and one out edge for each node, each of our traversals forms a chain.
During a traversal, if we hit a node which is part of a previous chain, then we do not need to put a new trap, because we have already put a trap
at the end of that chain during our previous iteration. Or, if we hit a node which a part of the current chain, then it means we have formed 
a cycle, and we need to put a new trap at the current point.

**Source Code:**
{% highlight C++ %}
#include <iostream>
#include <cstdio>
#include <vector>
#include <utility>

using namespace std;

vector<vector<pair<int, int>>> adjTable;
vector<vector<int>> dfs_num;

int trap = 0;

int iteration = 0;

void DFS(int r, int c){
    dfs_num[r][c] = iteration;
    
    int u = adjTable[r][c].first, v = adjTable[r][c].second;
    if (dfs_num[u][v] == iteration)
        trap++;
    else if (dfs_num[u][v] == -1)
        DFS(u, v);
}

int main(){
    int n, m;
    scanf("%d %d", &n, &m);

    adjTable.assign(n, vector<pair<int, int>>(m));
    dfs_num.assign(n, vector<int>(m, -1));

    for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++){
            char ch;
            scanf(" %c ", &ch);
            if (ch == 'N')
                adjTable[i][j] = {i - 1, j};
            else if (ch == 'S')
                adjTable[i][j] = {i + 1, j};
            else if (ch == 'W')
                adjTable[i][j] = {i, j - 1};
            else
                adjTable[i][j] = {i, j + 1};
        }
    
    for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++)
            if (dfs_num[i][j] == -1){
                DFS(i, j);
                iteration++;
            }
            
    printf("%d\n", trap);
}

{% endhighlight %}

[Herding]: http://www.spoj.com/problems/HERDING/
