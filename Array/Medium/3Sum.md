https://leetcode.com/problems/3sum/description/


#### Approach-1 Using Set &rarr; 437ms

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {

        int target = 0;
        int n = nums.length;

        //to avoid duplicates
        Arrays.sort(nums);

        List<List<Integer>> result = new ArrayList<>();

        for (int i = 0; i < n - 2 && nums[i] <= 0; i++) {

            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            Set numSet = new HashSet<>();
            target = 0 - nums[i];

            for (int j = i + 1; j < n; j++) {

                int complement = target - nums[j];

                if (numSet.contains(complement)) {
                    result.add(Arrays.asList(nums[i], complement, nums[j]));

                    while (j < n - 1 && nums[j] == nums[j + 1]) {
                        j++;
                    }
                }
                numSet.add(nums[j]);
            }
        }

        return result;
    }
}
```

#### Approach-2 Using 2 pointer &rarr; 30ms

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {

        int target = 0;
        int n = nums.length;

        Arrays.sort(nums);

        List<List<Integer>> result = new ArrayList<>();

        /*
            why nums[i]<=0 in for loop, bcz array already sorted
            next element will be grater only, so sum can not be 0
        */
        for (int i = 0; i < n - 2 && nums[i] <=0; i++) {

            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            target = 0 - nums[i];
            int j = i + 1, k = n - 1;
            while (j < k) {
                if (nums[j] + nums[k] == target) {
                    result.add(Arrays.asList(nums[i], nums[j], nums[k]));

                    j++;
                    k--;

                    while (j < k && nums[j] == nums[j - 1]) {
                        j++;
                    }

                    while (j < k && nums[k] == nums[k + 1]) {
                        k--;
                    }

                } else if (nums[j] + nums[k] > target) {
                    k--;
                } else if (nums[j] + nums[k] < target) {
                    j++;
                }
            }
        }

        return result;
    }
}
```