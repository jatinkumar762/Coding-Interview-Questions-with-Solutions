[Problem](https://leetcode.com/problems/maximum-subarray/)

* Kadane's Algorithm is a well-known algorithm used to solve the "Maximum Subarray Sum Problem"
* the goal is to find the contiguous subarray within a one-dimensional array of numbers that has the largest sum.

### Print subarray with maximum subarray sum

```java
class Solution {
    public int maxSubArray(int[] nums) {

        int left = 0, right = 0;
        int n = nums.length;
        int sum = 0, maxSum = 0;
        for (; right < n; right++) {

            if (right == 0) {
                sum = nums[right];
                maxSum = sum;
                continue;
            }

            if (sum + nums[right] < nums[right]) {
                left = right;
                sum = nums[right];
            } else {
                sum += nums[right];
            }

            maxSum = Math.max(sum, maxSum);
        }
        return maxSum;
    }
}
```

**Note**

* not asked range, so we can remove left pointer

```java
class Solution {
    public int maxSubArray(int[] nums) {

        int maxSum = nums[0], sum = nums[0];

        for (int i = 1; i < nums.length; i++) {

            sum += nums[i];

            if (sum < nums[i]) {
                sum = nums[i];
            }

            if (maxSum < sum) {
                maxSum = sum;
            }
        }

        return maxSum;
    }
}
```

**Time Complexity:** O(n), because the algorithm makes a single pass through the array.

**Space Complexity:** O(1), because it uses only a fixed amount of extra space.
