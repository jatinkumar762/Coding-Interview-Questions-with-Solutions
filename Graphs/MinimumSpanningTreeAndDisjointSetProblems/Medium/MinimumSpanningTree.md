
* A **spanning tree** is defined as a tree-like subgraph of a connected, undirected graph that includes all the vertices of the graph. or it is a subset of the edges of the graph that forms a tree (acyclic) where every node of the graph is a part of the tree.
* A **minimum spanning tree (MST)**  is defined as a spanning tree that has the minimum weight among all the possible spanning trees
* Like a spanning tree, there can also be many possible MSTs for a graph.
* The number of vertices (V) in the graph and the spanning tree is the same.
* ( E = V-1 )
* The spanning tree should not be disconnected, as in there should only be a single source of component, not more than that.
* The spanning tree should be acyclic, which means there would not be any cycle in the tree.
* The total cost (or weight) of the spanning tree is defined as the sum of the edge weights of all the edges of the spanning tree.
* There can be many possible spanning trees for a graph.

### Prim’s Minimum Spanning Tree Algorithm

* a greedy algorithm
* designed for undirected graphs only.

**Steps:**

* It starts by selecting an arbitrary vertex and then adding it to the MST.
* Then, it repeatedly checks for the minimum edge weight that connects one vertex of MST to another vertex that is not yet in the MST. 
* This process is continued until all the vertices are included in the MST. 

&rarr; this algorithm uses priority_queue to store the vertices sorted by their minimum edge weight currently. 

#### Problem

* https://www.geeksforgeeks.org/problems/minimum-spanning-tree/1

```java
class Edge{
    int u;
    int v;
    int weight;
    
    Edge(int u, int v, int w){
        this.u = u;
        this.v = v;
        this.weight = w;
    }
}

class Solution {
    static int spanningTree(int V, int E, List<List<int[]>> adj) {
        // Code Here.
        
        int sum = 0;
        boolean[] visited = new boolean[V];
        
        Comparator<Edge> edgeComparator = new Comparator<Edge>(){
            @Override
            public int compare(Edge e1, Edge e2){
                return Integer.compare(e1.weight, e2.weight);
            }
        };
        
        PriorityQueue<Edge> pq = new PriorityQueue<Edge>(edgeComparator);
        pq.add(new Edge(0, 0, 0));
        
        while(!pq.isEmpty()){
            
            Edge edge = pq.poll();
            int vertex = edge.v;
            
            if(visited[vertex]){
                continue;
            }
            
            //e[0] -> dest
            //e[1] -> weight
            for(int[] e : adj.get(vertex)){
                if(!visited[e[0]]){
                    pq.add(new Edge(vertex, e[0], e[1]));
                }
            }
            visited[vertex] = true;
            sum += edge.weight;
        }
        
        return sum;
    }
}
```

#### TimeComplexity:  `O(E log E)`

* In the worst case, the priority queue could hold up to O(E) edges.
* Inserting/Extracting an edge into the priority queue (pq.add()/pq.poll()) has a time complexity of O(log E).
* Since there are O(E) edges and each edge might be inserted and removed once, the priority queue operations contribute O(E log E) to the time complexity.

&rarr; However, because the number of edges E can be at most V^2 (in a dense graph), and since log(E) is at most log(V^2) = 2 log(V), we can simplify the time complexity to O(E log V) in most cases where V is the number of vertices. This matches the standard time complexity for Prim's algorithm using a priority queue (min-heap).

&rarr; **Dense graph** is a graph in which the number of edges is close to the maximal number of edges. **Sparse graph** is a graph in which the number of edges is close to the minimal number of edges. Sparse graph can be a disconnected graph.


#### Reference:

- https://www.geeksforgeeks.org/what-is-minimum-spanning-tree-mst/