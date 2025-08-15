https://leetcode.com/problems/tree-diameter/description/


### Approach 1: Farthest Nodes via BFS

* Given any node, the longest distance that starts from this node must end with one of the extreme peripheral nodes.
* Once we identify one of the extreme peripheral nodes, we then could apply again the BFS traversal.

```java
class Solution {
    public int treeDiameter(int[][] edges) {

        int n = edges.length + 1;

        List<Set<Integer>> graph = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            graph.add(new HashSet<>());
        }

        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
        }

        int farthest[] = findFarthestNode(0, graph, n);

        int maxDist[] = findFarthestNode(farthest[0], graph, n);

        return maxDist[1];
    }

    private int[] findFarthestNode(int start, List<Set<Integer>> graph, int n) {

        boolean[] visited = new boolean[n];

        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[] { start, 0 });

        visited[start] = true;

        int[] max = new int[] { start, 0 };

        while (!queue.isEmpty()) {

            int[] node = queue.poll();

            Iterator<Integer> itr = graph.get(node[0]).iterator();

            while (itr.hasNext()) {

                int nbr = itr.next();

                if (!visited[nbr]) {

                    visited[nbr] = true;

                    queue.add(new int[] { nbr, node[1] + 1 });

                    if (max[1] < (node[1] + 1)) {

                        max = new int[] { nbr, node[1] + 1 };

                    }
                }

            }
        }

        return max;
    }
}
```