https://leetcode.com/problems/non-overlapping-intervals/description/

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {

        int N = intervals.length;

        //minimum number of intervals
        //so sorting based on end time
        //(1, 40) (2, 6) (7, 10) (1, 12)

        Arrays.sort(intervals, (a, b) -> a[1] - b[1]);

        int remove = 0;
        int end = intervals[0][1];
        for (int i = 1; i < N; i++) {
            // check overlap
            if (intervals[i][0] < end) {
                remove += 1;
            } else {
                end = intervals[i][1];
            }
        }

        return remove;
    }
}
```