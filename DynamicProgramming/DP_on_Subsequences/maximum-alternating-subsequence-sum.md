https://leetcode.com/problems/maximum-alternating-subsequence-sum/description/

### Recursion

**Top-down**

```java
class Solution {
    public long maxAlternatingSum(int[] nums) {

        int len = nums.length;

        return findMaxSum(nums, len - 1, 1);
    }

    private int findMaxSum(int[] nums, int index, int sign) {

        if (index < 0) {
            return 0;
        }

        //if current element positive, then next will be negative in subsequence
        int take = sign * nums[index] + findMaxSum(nums, index - 1, -1 * sign);

        //if do not take current element, then sign will not change
        int notTake = findMaxSum(nums, index - 1, sign);

        return Math.max(take, notTake);
    }
}
```

**Bottom-up**

```
class Solution {
    public long maxAlternatingSum(int[] nums) {

        int len = nums.length;

        return findMaxSum(nums, 0, len, 1, 0);
    }

    private int findMaxSum(int[] nums, int start, int end, int sign, int sum) {

        if (start == end) {
            return sum;
        }

        int take = findMaxSum(nums, start + 1, end, -1 * sign, sign * nums[start] + sum);

        int notTake = findMaxSum(nums, start + 1, end, sign, sum);

        return Math.max(take, notTake);
    }
}
```

### Top-Down (Memorization)

i is the current index in the array.

s is the sign (+1 or -1).

Now, for any i and s, you have two choices:

Take nums[i]: The current value will be added with sign s, and the next call will be with -s:

take = s * nums[i] + dp(i-1, -s)

Skip nums[i]: The sign remains the same, and you move to the previous index:

notTake = dp(i-1, s)

the recurrence relation is:

dp(i, s) = max(s * nums[i] + dp(i-1, -s), dp(i-1, s))


```java
class Solution {
    public long maxAlternatingSum(int[] nums) {

        int len = nums.length;

        long[][] dp = new long[len][2];

        for (long[] row : dp) {
            Arrays.fill(row, -1L);
        }

        return findMaxSum(nums, len - 1, 0, dp);
    }

    private long findMaxSum(int[] nums, int index, int sign, long[][] dp) {

        if (index < 0) {
            return 0;
        }

        if (dp[index][sign] != -1L) {
            return dp[index][sign];
        }

        int mulSign = sign == 0 ? 1 : -1;

        long take = mulSign * nums[index] + findMaxSum(nums, index - 1, 1 - sign, dp);

        long notTake = findMaxSum(nums, index - 1, sign, dp);

        return dp[index][sign] = Math.max(take, notTake);
    }
}
```

### Bottom-Up (Tabulation)

```java
class Solution {
    public long maxAlternatingSum(int[] nums) {

        int len = nums.length;

        long[][] dp = new long[len][2];

        //even - 0 index +ve
        //odd - 1 index -ve

        dp[0][0] = nums[0];
        dp[0][1] = 0;

        for (int i = 1; i < len; i++) {

            //even index -> will be postive
            dp[i][0] = Math.max(nums[i] + dp[i-1][1], dp[i - 1][0]);

            //odd index -> will be negative
            dp[i][1] = Math.max(dp[i - 1][0] - nums[i], dp[i - 1][1]);
        }

        //max will be at even index, but perform addition here
        return dp[len-1][0];
    }
}
```

**Optimization**

```java
class Solution {
    public long maxAlternatingSum(int[] nums) {

        int len = nums.length;

        long[][] dp = new long[len][2];

        //even - 0 index +ve
        //odd - 1 index -ve

        long even = nums[0];
        long odd = 0;

        for (int i = 1; i < len; i++) {

            //even index -> will be postive
            long newEven = Math.max(nums[i] + odd, even);

            //odd index -> will be negative
            long newOdd = Math.max(even - nums[i], odd);

            even = newEven;
            odd = newOdd;
        }

        //max will be at even index, but perform addition here
        return even;
    }
}
```