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

```java

```