https://leetcode.com/problems/minimum-size-subarray-sum/description/


```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {

        int left = 0;
        int right = 0;
        int N = nums.length;

        int sum = 0;
        int min = Integer.MAX_VALUE;

        while (right < N) {

            sum += nums[right];

            if (sum >= target) {
                min = Math.min(right - left + 1, min);
            }

            while (sum >= target && left < right) {

                sum -= nums[left];

                left++;

                if (sum >= target) {
                    min = Math.min(right - left + 1, min);
                }
            }

            right++;
        }

        return min == Integer.MAX_VALUE ? 0 : min;
    }
}
```