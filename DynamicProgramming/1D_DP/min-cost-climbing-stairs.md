https://leetcode.com/problems/min-cost-climbing-stairs/description/

### Bottom Up

```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {

        int N = cost.length;

        int[] res = new int[N + 1];
        //default value is 0
        //Arrays.fill(res, 0);

        // res[0] = cost[0];
        // res[1] = cost[1];

        for (int i = 2; i <= N; i++) {
            res[i] = Math.min(res[i - 1] + cost[i - 1], res[i - 2] + cost[i - 2]);
        }

        return res[N];
    }
}
```

### Top-Down Dynamic Programming (Recursion + Memoization)