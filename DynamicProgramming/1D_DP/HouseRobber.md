https://leetcode.com/problems/house-robber/description/

### Recursive Solution - TLE

```java
class Solution {
    public int rob(int[] nums) {

        int n = nums.length;

        if (n == 1) {
            return nums[0];
        }

        if (n == 2) {
            return nums[0] > nums[1] ? nums[0] : nums[1];
        }

        return findSum(nums, 0, n);
    }

    private int findSum(int[] nums, int index, int n) {

        if (index >= n) {
            return 0;
        }

        if (index == n - 1) {
            return nums[index];
        }

        int take = nums[index] + findSum(nums, index + 2, n);

        int notTake = findSum(nums, index + 1, n);

        return take > notTake ? take : notTake;
    }
}
```

### Top-Down Approach (Similar to generate subsequence)

```java
class Solution {
    public int rob(int[] nums) {

        int n = nums.length;

        if (n == 1) {
            return nums[0];
        }

        if (n == 2) {
            return nums[0] > nums[1] ? nums[0] : nums[1];
        }

        int[] dp = new int[n];
        Arrays.fill(dp, -1);

        findSum(nums, dp, n - 1);

        return dp[n - 1];
    }

    private int findSum(int[] nums, int[] dp, int n) {

        if (n < 0) {
            return 0;
        }

        if (n == 0) {
            return dp[n] = nums[n];
        }

        if (dp[n] != -1) {
            return dp[n];
        }

        int take = nums[n] + findSum(nums, dp, n - 2);

        int notTake = findSum(nums, dp, n - 1);

        return dp[n] = take > notTake ? take : notTake;
    }
}
```

### Bottom-Up Approach

[2, 1, 1, 2] &rarr; 4

```java
class Solution {

    int[] dp;

    public int rob(int[] nums) {

        int N = nums.length;

        if (N == 1) {
            return nums[0];
        } else if (N == 2) {
            return nums[0] < nums[1] ? nums[1] : nums[0];
        }

        dp = new int[N];

        dp[0] = nums[0];
        dp[1] = nums[1] > nums[0] ? nums[1] : nums[0]; //max rob between index [0 .. 1]

        for (int i = 2; i < N; i++) {

            int notTake = dp[i - 1];

            int take = nums[i] + dp[i - 2];

            dp[i] = take > notTake ? take : notTake;
        }

        return dp[N - 1];
    }
}
```

**Space Optimization**

```java
class Solution {

    public int rob(int[] nums) {

        int N = nums.length;

        if (N == 1) {
            return nums[0];
        } else if (N == 2) {
            return nums[0] < nums[1] ? nums[1] : nums[0];
        }

        int prev2 = nums[0];
        int prev = nums[1] > nums[0] ? nums[1] : nums[0];

        for (int i = 2; i < N; i++) {

            int notTake = prev;

            int take = nums[i] + prev2;

            int max = take > notTake ? take : notTake;

            prev2 = prev;
            prev = max;
        }

        return prev;
    }
}
```