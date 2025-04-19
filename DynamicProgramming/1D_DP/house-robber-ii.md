https://leetcode.com/problems/house-robber-ii/description/




```java
class Solution {
    public int rob(int[] nums) {
        
        int N = nums.length;

        if (N == 1) {
            return nums[0];
        } else if (N == 2) {
            return nums[0] < nums[1] ? nums[1] : nums[0];
        }

        //0 and N-1 can not be in answer because of circular 
        //leave 0 and found max
        //leave N-1 and foudn max

        int case1 = calculateMaxAmount(nums, 0, N-2);

        int case2 = calculateMaxAmount(nums, 1, N-1);

        return case1 > case2 ? case1 : case2;
    }

    private int calculateMaxAmount(int[] nums, int start, int end){

        int[] dp = new int[end + 1];

        dp[start] = nums[start];
        dp[start + 1] = nums[start + 1] > nums[start] ? nums[start + 1] : nums[start]; //max rob between index [0 .. 1]

        for (int i = start + 2; i <= end; i++) {

            int notTake = dp[i - 1];

            int take = nums[i] + dp[i - 2];

            dp[i] = take > notTake ? take : notTake;
        }

        return dp[end];
    }
}
```