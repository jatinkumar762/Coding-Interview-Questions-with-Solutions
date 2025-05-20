https://leetcode.com/problems/find-k-pairs-with-smallest-sums/description/


### TLE Solution

```java
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {

        Comparator<Pair<Integer, Integer>> comparator = new Comparator<Pair<Integer, Integer>>() {

            @Override
            public int compare(Pair<Integer, Integer> p1, Pair<Integer, Integer> p2) {

                return -1 * Integer.compare(p1.getKey() + p1.getValue(), p2.getKey() + p2.getValue());

            }
        };

        PriorityQueue<Pair<Integer, Integer>> pq = new PriorityQueue<>(comparator);

        int len1 = nums1.length;
        int len2 = nums2.length;

        int size = 1;
        for (int i = 0; i < len1; i++) {

            for (int j = 0; j < len2; j++) {

                pq.add(new Pair(nums1[i], nums2[j]));

                if (size > k) {
                    pq.poll();
                } else {
                    size++;
                }
            }
        }

        List<List<Integer>> res = new ArrayList<>();

        while (!pq.isEmpty()) {
            Pair<Integer, Integer> p = pq.poll();

            res.add(Arrays.asList(p.getKey(), p.getValue()));
        }

        return res;
    }
}
```

### Optimal Solution

```java
/*
[1,2,4,5,6]
[3,5,7,9]


(0,0) - (0, 1)
        (1, 0)

(0, 1) - (1, 1)
         (0, 2)

(1, 0) - (2, 0)
         (1, 1)

two times (1, 1) -> duplicate check required
*/


class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        int m = nums1.length;
        int n = nums2.length;

        List<List<Integer>> ans = new ArrayList<>();
        Set<Pair<Integer, Integer>> visited = new HashSet<>();

        PriorityQueue<int[]> minHeap = new PriorityQueue<>((a, b) -> (a[0] - b[0]));
        minHeap.offer(new int[] { nums1[0] + nums2[0], 0, 0 });
        visited.add(new Pair<Integer, Integer>(0, 0));

        while (k-- > 0 && !minHeap.isEmpty()) {
            int[] top = minHeap.poll();
            int i = top[1];
            int j = top[2];

            ans.add(List.of(nums1[i], nums2[j]));

            if (i + 1 < m && !visited.contains(new Pair<Integer, Integer>(i + 1, j))) {
                minHeap.offer(new int[] { nums1[i + 1] + nums2[j], i + 1, j });
                visited.add(new Pair<Integer, Integer>(i + 1, j));
            }

            if (j + 1 < n && !visited.contains(new Pair<Integer, Integer>(i, j + 1))) {
                minHeap.offer(new int[] { nums1[i] + nums2[j + 1], i, j + 1 });
                visited.add(new Pair<Integer, Integer>(i, j + 1));
            }
        }

        return ans;
    }
}
```