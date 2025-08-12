https://leetcode.com/problems/minimum-height-trees/description/


### Brute Force Solution

```java
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {

        List<List<Integer>> graph = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<Integer>());
        }

        for (int[] edge : edges) {

            graph.get(edge[0]).add(edge[1]);

            graph.get(edge[1]).add(edge[0]);
        }

        List<Integer> result = new ArrayList<>();
        int minHeight = Integer.MAX_VALUE;
        for (int i = 0; i < n; i++) {

            int height = height(i, graph, n);

            if (minHeight == height) {
                result.add(i);
            } else if (minHeight > height) {
                minHeight = height;
                result.clear();
                result.add(i);
            }

        }

        return result;
    }

    private int height(int root, List<List<Integer>> graph, int n) {

        boolean[] visited = new boolean[n];

        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[] { root, 0 });

        visited[root] = true;

        int height = 0;

        while (!queue.isEmpty()) {

            int[] node = queue.poll();

            for (Integer nbr : graph.get(node[0])) {

                if (!visited[nbr]) {
                    queue.add(new int[] { nbr, node[1] + 1 });

                    height = Math.max(height, node[1] + 1);

                    visited[nbr] = true;
                }
            }

        }

        return height;
    }
}
```

### Topological Sorting

```java
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {

        if (n == 1) return Collections.singletonList(0);

        List<Set<Integer>> graph = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            graph.add(new HashSet<Integer>());
        }

        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
        }

        List<Integer> leafNodes = new ArrayList<>();

        for (int i = 0; i < n; i++) {

            if (graph.get(i).size() <= 1) {
                leafNodes.add(i);
            }
        }


        while (n > 2) {

            n -= leafNodes.size();

            List<Integer> newLeafs = new ArrayList<>();

            for (Integer leaf : leafNodes) {

                int nbr = graph.get(leaf).iterator().next();

                graph.get(nbr).remove(leaf);

                if (graph.get(nbr).size() == 1) {
                    newLeafs.add(nbr);
                }
            }

            leafNodes = newLeafs;
        }

        return leafNodes;
    }
}
```
