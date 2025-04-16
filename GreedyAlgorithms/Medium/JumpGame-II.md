https://leetcode.com/problems/jump-game-ii/description/

* here we need to determine, maxJump can go
* do next jump calculation, after checking all index of current maxJump and increase count +1

### Approach-1 `1ms`

&rarr; here we can not track position, from where he jumped

```java
class Solution {
    public int jump(int[] nums) {
        int N = nums.length - 1;
        int count = 0;
        int position = 0;
        int maxIndex = position + nums[position];

        // if start at N-1, means reached at destination
        while (position < N) {

            count++;

            if (maxIndex >= N) {
                break;
            }

            int newIndex = maxIndex;
            int i;
            for (i = position + 1; i <= maxIndex; i++) {
                newIndex = Math.max(newIndex, i+nums[i]);
            }

            position = i - 1; //bcz i is maxIndex+1
            maxIndex = newIndex;
        }

        return count;
    }
}
```

**other way to write - 1ms **

&rarr; here we can track position also, from where he jumped

`1ms`

```java
class Solution {
    public int jump(int[] nums) {
        int N = nums.length - 1;
        int count = 0;
        int position = 0;

        // if start at N-1, means reached at destination
        while (position < N) {

            count++;

            if (position + nums[position] >= N) {
                break;
            }

            int maxJumpPosition = position + nums[position];
            int max = 0;
            for (int i = position + 1; i <= maxJumpPosition; i++) {
                int current = i + nums[i];
                if(current>max){
                    max = current;
                    position = i;
                }
            }
        }

        return count;
    }
}
```

**other way to write - 0ms solution **

```java
class Solution {
        public int jump(int[] nums) {
        int jumps = 0;
        int lastIndex = nums.length - 1;
        int i = 0;
        while (i < lastIndex) {
            jumps++;
            if (i + nums[i] >= lastIndex) break; // final jump possible
            i = maxPositionOfJump(nums, i);
        }
        return jumps;
    }

    private int maxPositionOfJump(int[] nums, int position) {
        int maxJumpPosition = position + nums[position];
        int max = 0;
        for (int i = position + 1; i <= maxJumpPosition; i++) {
            int current = i + nums[i];
            if (current >= max) {
                position = i;
                max = current;
            }
        }
        return position;
    }
}
```

### Approach-2 Basic

```java
class Solution {
    public int jump(int[] nums) {

        int n = nums.length;

        int[] count = new int[n];
        Arrays.fill(count, -1);

        count[0] = 0;

        for (int i = 0; i < n; i++) {

            for (int j = 1; (i + j) < n && j <= nums[i]; j++) {

                if (count[i + j] == -1) {
                    count[i + j] = count[i] + 1;
                } else {
                    count[i + j] = Math.min(count[i] + 1, count[i + j]);
                }
            }
        }

        return count[n - 1];
    }
}
```