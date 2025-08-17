https://leetcode.com/problems/target-sum/description/

* this problem is extension of Problem DP - Perfect Sum Problem

1. S1 + S2 = total
2. S1 - S2 = target

step 1 - step 2 &rarr; S2 = (total - target)/2

will do count of S2 &rarr; then we can get count of S1 - S2 = target

```java
class Solution {

    int modulo = 1000000007;
    int[][] dp;

    public int perfectSum(int arr[], int n, int sum) {
        dp = new int[n + 1][sum + 1];

        // base case initialization
        for (int i = 0; i < n; i++) {
            dp[i][0] = 1; // empty subset
        }

        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= sum; j++) {
                int not_take = dp[i - 1][j]; // not take

                int take = 0;
                if (arr[i - 1] <= j) {
                    take = dp[i - 1][j - arr[i - 1]]; // take
                }

                dp[i][j] = (not_take + take) % modulo;
            }
        }

        return dp[n][sum];
    }

    public int findTargetSumWays(int[] nums, int target) {

        int sum = 0;
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            sum += nums[i];
        }

        if (target > sum) {
            //from nums elements we can not make target
            //even if all +
            return 0;
        }

        if ((sum - target) % 2 != 0) {
            return 0;
        }

        return perfectSum(nums, n, (sum - target) / 2);
    }
}
```