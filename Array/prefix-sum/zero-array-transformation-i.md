https://leetcode.com/problems/zero-array-transformation-i/

### TLE Solution

```java
class Solution {
    public boolean isZeroArray(int[] nums, int[][] queries) {

        int len = nums.length;

        int qLen = queries.length;

        for (int i = 0; i < qLen; i++) {

            for (int j = queries[i][0]; j <= queries[i][1]; j++) {

                if (nums[j] > 0) {
                    nums[j] -= 1;
                }
            }
        }

        for (int i = 0; i < len; i++) {
            if (nums[i] != 0) {
                return false;
            }
        }
        return true;
    }
}
```

### Optimized Solution

```java
class Solution {
    public boolean isZeroArray(int[] nums, int[][] queries) {

        int len = nums.length;

        int qLen = queries.length;

        int[] deltaArray = new int[len + 1];

        for (int i = 0; i < qLen; i++) {

            int left = queries[i][0];
            int right = queries[i][1];

            deltaArray[left] += 1;
            deltaArray[right + 1] -= 1;
        }
        /*
            why doing right+1 index -=1
            bcz when we do prefix sum, from right+1 index operation not counted 
        
        
            otherwise, we need to follow basic approach
            for each query range, i need to initialize operation count
        */

        //prefix sum
        //count num of operations on each index
        int[] operationsCount = new int[len + 1]; //actual operation counts at each position 
        int count = 0;
        for (int i = 0; i < len; i++) {
            count += deltaArray[i];
            operationsCount[i] = count;
        }

        for (int i = 0; i < len; i++) {
            if (nums[i] > operationsCount[i]) {
                return false;
            }
            //else we can make zero
        }
        return true;
    }
}
```