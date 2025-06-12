https://leetcode.com/problems/container-with-most-water/description/

### Basic approach

```java
class Solution {
    public int maxArea(int[] height) {

        int len = height.length;
        int max = 0;

        for (int i = 0; i < len; i++) {

            for (int j = i + 1; j < len; j++) {

                int min = Math.min(height[i], height[j]);

                max = Math.max(min * (j - i), max);
            }
        }

        return max;
    }
}
```

### 2-pointer approach

```java
class Solution {
    public int maxArea(int[] height) {

        int len = height.length;

        int left = 0;
        int right = len - 1;

        int max = 0;
        int minHeight;

        while (left < right) {

            minHeight = Math.min(height[left], height[right]);

            max = Math.max(max, (right - left) * minHeight);

            if (height[left] <= height[right]) {
                left++;
            } else {
                right--;
            }
        }

        return max;
    }
}
```