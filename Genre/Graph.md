# Graph

## Introduce

### Types of “graphs”

There are many types of “graphs”. In this Explore Card, we will introduce three types of graphs: **undirected graphs, directed graphs, and weighted graphs.**

### The Definition of “graph” and Terminologies

“Graph” is a non-linear data structure consisting of vertices and edges. There are a lot of terminologies to describe a graph. If you encounter an unfamiliar term in the following Explore Card, you may look up the definition below.

* Vertex: In Figure 1, nodes such as A, B, and C are called vertices of the graph.
* Edge: The connection between two vertices are the edges of the graph. In Figure 1, the connection between person A and B is an edge of the graph.
* Path: the sequence of vertices to go through from one vertex to another. In Figure 1, a path from A to C is [A, B, C], or [A, G, B, C], or [A, E, F, D, B, C].**Note**: there can be multiple paths between two vertices.
* Path Length: the number of edges in a path. In Figure 1, the path lengths from person A to C are 2, 3, and 5, respectively.
* Cycle: a path where the starting point and endpoint are the same vertex. In Figure 1, [A, B, D, F, E] forms a cycle. Similarly, [A, G, B] forms another cycle.
* Negative Weight Cycle: In a “weighted graph”, if the sum of the weights of all edges of a cycle is a negative value, it is a negative weight cycle. In Figure 4, the sum of weights is -3.
* Connectivity: if there exists at least one path between two vertices, these two vertices are connected. In Figure 1, A and C are connected because there is at least one path connecting them.
* Degree of a Vertex: the term “degree” applies to unweighted graphs. The degree of a vertex is the number of edges connecting the vertex. In Figure 1, the degree of vertex A is 3 because three edges are connecting it.
* In-Degree: “in-degree” is a concept in directed graphs. If the in-degree of a vertex is d, there are d directional edges incident to the vertex. In Figure 2, A’s indegree is 1, i.e., the edge from F to A.
* Out-Degree: “out-degree” is a concept in directed graphs. If the out-degree of a vertex is d, there are d edges incident from the vertex. In Figure 2, A’s outdegree is 3, i,e, the edges A to B, A to C, and A to G.

## Disjoint Set

### The two important functions of a “disjoint set.”

---

In the introduction videos above, we discussed the two important functions in a “disjoint set”.

* **The `find` function** finds the root node of a given vertex. For example, in Figure 5, the output of the find function for vertex 3 is 0.
* **The `union` function** unions two vertices and makes their root nodes the same. In Figure 5, if we union vertex 4 and vertex 5, their root node will become the same, which means the union function will modify the root node of vertex 4 or vertex 5 to the same root node.

### There are two ways to implement a “disjoint set”.

---

* Implementation with Quick Find: in this case, the time complexity of the `find` function will be **O**(**1**). However, the `union` function will take more time with the time complexity of **O**(**N**).
* Implementation with Quick Union: compared with the Quick Find implementation, the time complexity of the `union` function is better. Meanwhile, the `find` function will take more time in this case.

## Code Example

### 547. Number of Province | Medium

There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`.

A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = 1` if the `i<sup>th</sup>` city and the `j<sup>th</sup>` city are directly connected, and `isConnected[i][j] = 0` otherwise.

Return  *the total number of **provinces*** .

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg)

<pre><strong>Input:</strong> isConnected = [[1,1,0],[1,1,0],[0,0,1]]
<strong>Output:</strong> 2
</pre>

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/24/graph2.jpg)

<pre><strong>Input:</strong> isConnected = [[1,0,0],[0,1,0],[0,0,1]]
<strong>Output:</strong> 3</pre>

#### 分析

Using Union-Find Method

#### 基本思路

Another method that can be used to determine the number of connected components in a graph is the union find method. The method is simple.

We make use of a parent array of size **N**. We traverse over all the nodes of the graph. For every node traversed, we traverse over all the nodes directly connected to it and assign them to a single group which is represented by their **p**a**r**e**n**t node. This process is called forming a **u**n**i**o**n**. Every group has a single **p**a**r**e**n**t node, whose own parent is given by \text{-1}**-1**.

For every new pair of nodes found, we look for the parents of both the nodes. If the parents nodes are the same, it indicates that they have already been united into the same group. If the parent nodes differ, it means they are yet to be united. Thus, for the pair of nodes **(**x**,**y**)**, while forming the union, we assign parent\big[parent[x]\big]=parent[y]**p**a**r**e**n**t**[**p**a**r**e**n**t**[**x**]**]**=**p**a**r**e**n**t**[**y**]**, which ultimately combines them into the same group.

* Time complexity : O(n^3). We traverse over the complete matrix once. Union and find operations take O(n)**O**(**n**) time in the worst case.
* Space complexity : O(n). **p**a**r**e**n**t array of size n**n** is used

#### JAVA

```java
public class Solution {
    int find(int parent[], int i) {
        if (parent[i] == -1)
            return i;
        return find(parent, parent[i]);
    }

    void union(int parent[], int x, int y) {
        int xset = find(parent, x);
        int yset = find(parent, y);
        if (xset != yset)
            parent[xset] = yset;
    }
    public int findCircleNum(int[][] M) {
        int[] parent = new int[M.length];
        Arrays.fill(parent, -1);
        for (int i = 0; i < M.length; i++) {
            for (int j = 0; j < M.length; j++) {
                if (M[i][j] == 1 && i != j) {
                    union(parent, i, j);
                }
            }
        }
        int count = 0;
        for (int i = 0; i < parent.length; i++) {
            if (parent[i] == -1)
                count++;
        }
        return count;
    }
}
```
