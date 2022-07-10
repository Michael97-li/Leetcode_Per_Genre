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
class Solution {
    // Union Find
    public int findCircleNum(int[][] isConnected) {
        if (isConnected == null || isConnected.length == 0) {
            return 0;
        }

        int n = isConnected.length;
        UnionFind uf = new UnionFind(n);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (isConnected[i][j] == 1) {
                    uf.union(i, j);
                }
            }
        }

        return uf.getCount();
    }

    class UnionFind {
        private int[] root;
        private int[] rank;
        private int count;

        UnionFind(int size) {
            root = new int[size];
            rank = new int[size];
            count = size;
            for (int i = 0; i < size; i++) {
                root[i] = i;
                rank[i] = 1;
            }
        }

        int find(int x) {
            if (x == root[x]) {
                return x;
            }
            return root[x] = find(root[x]);
        }

        void union(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);
            if (rootX != rootY) {
                if (rank[rootX] > rank[rootY]) {
                    root[rootY] = rootX;
                } else if (rank[rootX] < rank[rootY]) {
                    root[rootX] = rootY;
                } else {
                    root[rootY] = rootX;
                    rank[rootX] += 1;
                }
                count--;
            }
        }

        int getCount() {
            return count;
        }
    }
}
```

### 261. Graph Valid Tree | Medium

You have a graph of `n` nodes labeled from `0` to `n - 1`. You are given an integer n and a list of `edges` where `edges[i] = [a<sub>i</sub>, b<sub>i</sub>]` indicates that there is an undirected edge between nodes `a<sub>i</sub>` and `b<sub>i</sub>` in the graph.

Return `true` *if the edges of the given graph make up a valid tree, and* `false`  *otherwise* .

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/12/tree1-graph.jpg)

<pre><strong>Input:</strong> n = 5, edges = [[0,1],[0,2],[0,3],[1,4]]
<strong>Output:</strong> true
</pre>

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/12/tree2-graph.jpg)

<pre><strong>Input:</strong> n = 5, edges = [[0,1],[1,2],[2,3],[1,3],[1,4]]
<strong>Output:</strong> false
</pre>

**Constraints:**

* `1 <= n <= 2000`
* `0 <= edges.length <= 5000`
* `edges[i].length == 2`
* `0 <= a<sub>i</sub>, b<sub>i</sub><span> </span>< n`
* `a<sub>i</sub><span> </span>!= b<sub>i</sub>`
* There are no self-loops or repeated edges

### 分析

Using Union-Find Method

#### 基本思路

For the graph to be a valid tree, it must have *exactly* `n - 1` edges. Any less, and it can't possibly be fully connected. Any more, and it *has* to contain cycles. Additionally, if the graph is fully connected *and* contains exactly `n - 1` edges, it can't *possibly* contain a cycle, and therefore must be a tree!

This definition simplified the problem down to checking whether or not the graph is fully connected. If it is, and if it contains `n - 1` edges, then we know it's a tree. In the previous approaches, we used graph search algorithms to check whether or not all nodes were reachable, starting from a single source node.

Each time there was no merge, it was because we were adding an edge between two nodes that were already connected via a path. This means there is now an additional path between them—which is the definition of a cycle. Therefore,  *as soon as this happens* , we can terminate the algorithm and return `false`.

* Time complexity : O(N⋅α(N))
* Space complexity : O(n)

#### JAVA

```java
class Solution {
    // Union Find
    public int findCircleNum(int[][] isConnected) {
        if (isConnected == null || isConnected.length == 0) {
            return 0;
        }

        int n = isConnected.length;
        UnionFind uf = new UnionFind(n);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (isConnected[i][j] == 1) {
                    uf.union(i, j);
                }
            }
        }

        return uf.getCount();
    }

    class UnionFind {
        private int[] root;
        private int[] rank;
        private int count;

        UnionFind(int size) {
            root = new int[size];
            rank = new int[size];
            count = size;
            for (int i = 0; i < size; i++) {
                root[i] = i;
                rank[i] = 1;
            }
        }

        int find(int x) {
            if (x == root[x]) {
                return x;
            }
            return root[x] = find(root[x]);
        }

        void union(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);
            if (rootX != rootY) {
                if (rank[rootX] > rank[rootY]) {
                    root[rootY] = rootX;
                } else if (rank[rootX] < rank[rootY]) {
                    root[rootX] = rootY;
                } else {
                    root[rootY] = rootX;
                    rank[rootX] += 1;
                }
                count--;
            }
        }

        int getCount() {
            return count;
        }
    }
}
```

### 323. Number of Connected Components in an Undirected Grap | Medium

You have a graph of `n` nodes. You are given an integer `n` and an array `edges` where `edges[i] = [a<sub>i</sub>, b<sub>i</sub>]` indicates that there is an edge between `a<sub>i</sub>` and `b<sub>i</sub>` in the graph.

Return  *the number of connected components in the graph* .

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/conn1-graph.jpg)

<pre><strong>Input:</strong> n = 5, edges = [[0,1],[1,2],[3,4]]
<strong>Output:</strong> 2
</pre>

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/conn2-graph.jpg)

<pre><strong>Input:</strong> n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]
<strong>Output:</strong> 1
</pre>

**Constraints:**

* `1 <= n <= 2000`
* `1 <= edges.length <= 5000`
* `edges[i].length == 2`
* `0 <= a<sub>i</sub><span> </span><= b<sub>i</sub><span> </span>< n`
* `a<sub>i</sub><span> </span>!= b<sub>i</sub>`
* There are no repeated edges.

### 分析

Using Union-Find Method

#### 基本思路

Imagine we have a graph with `N` vertices and `0` edges. The number of connected components will be `N` in that graph.

![fig](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/Figures/323/DSU-1.png)

Let's now add the edge from vertex 1 to vertex 2. This will decrease the number of components by 1. This is because vertices 1 and 2 are now in the same component.

![fig](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/Figures/323/DSU-2.png)

When we then add the edge from vertex 2 to vertex 3, the number of components will decrease by 1 again.

![fig](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/Figures/323/DSU-3.png)

However, this pattern will not continue when we add the edge from vertex 1 to vertex 3. The number of components will not change because vertices 1, 2, and 3 are already in the same component.

![fig](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/Figures/323/DSU-4.png)

The above observation is the main intuition behind the DSU approach.

**Algorithm**

1. Initialize a variable `count` with the number of vertices in the input.
2. Traverse all of the edges one by one, performing the union-find method `combine` on each edge. If the endpoints are already in the same set, then keep traversing. If they are not, then decrement `count` by 1.
3. After traversing all of the `edges`, the variable `count` will contain the number of components in the graph.


Here E**E** = Number of edges, V**V** = Number of vertices.

* Time complexity: O(E\cdotα(n))**O**(**E**⋅**α**(**n**)**)**.
  Iterating over every edge requires O(E)**O**(**E**) operations, and for every operation, we are performing the `combine` method which is O(α(n))**O**(**α**(**n**)**)**, where α(n) is the inverse Ackermann function.
* Space complexity: O(V)**O**(**V**).
  Storing the representative/immediate-parent of each vertex takes O(V)**O**(**V**) space. Furthermore, storing the size of components also takes O(V)**O**(**V**) space.

#### JAVA

```java
public class Solution {

    private int find(int[] representative, int vertex) {
        if (vertex == representative[vertex]) {
            return vertex;
        }
    
        return representative[vertex] = find(representative, representative[vertex]);
    }
  
    private int combine(int[] representative, int[] size, int vertex1, int vertex2) {
        vertex1 = find(representative, vertex1);
        vertex2 = find(representative, vertex2);
    
        if (vertex1 == vertex2) {
            return 0;
        } else {
            if (size[vertex1] > size[vertex2]) {
                size[vertex1] += size[vertex2];
                representative[vertex2] = vertex1;
            } else {
                size[vertex2] += size[vertex1];
                representative[vertex1] = vertex2;
            }
            return 1;
        }
    }

    public int countComponents(int n, int[][] edges) {
        int[] representative = new int[n];
        int[] size = new int[n];
    
        for (int i = 0; i < n; i++) {
            representative[i] = i;
            size[i] = 1;
        }
    
        int components = n;
        for (int i = 0; i < edges.length; i++) { 
            components -= combine(representative, size, edges[i][0], edges[i][1]);
        }

        return components;
    }
}
```
