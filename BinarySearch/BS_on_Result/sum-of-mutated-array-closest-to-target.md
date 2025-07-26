https://leetcode.com/problems/sum-of-mutated-array-closest-to-target/description/


### Binary Search

```java
/*
We know answer can be possible between this range - concept binary search
*/

class Solution {
    public int findBestValue(int[] arr, int target) {

        int len = arr.length;

        int min = 0;

        int max = Integer.MIN_VALUE;
        for (int i = 0; i < len; i++) {
            if (max < arr[i]) {
                max = arr[i];
            }
        }

        int minDiff = Integer.MAX_VALUE;
        int res = -1;

        while (min <= max) {

            int mid = min + (max - min) / 2;

            int sum = 0;
            for (int i = 0; i < len; i++) {

                if (arr[i] <= mid) {
                    sum += arr[i];
                } else {
                    sum += mid;
                }
            }

            //min integer
            if (Math.abs(target - sum) < minDiff) {
                res = mid;
                minDiff = Math.abs(target - sum);
            } else if (Math.abs(target - sum) == minDiff && mid < res) {
                res = mid;
            }

            if (sum < target) {
                min = mid + 1;
            } else {
                max = mid - 1;
            }
        }

        return res;
    }
}
```