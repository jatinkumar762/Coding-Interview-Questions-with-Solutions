https://practice.geeksforgeeks.org/problems/next-larger-element-1587115620/1

```java
class Solution
{
    //Function to find the next greater element for each element of the array.
    public static long[] nextLargerElement(long[] arr, int n)
    { 
        // Your code here
        Stack<Long> stack = new Stack<Long>();
        
        long[] result = new long[n];
        
        long max=0;
        int i;
        for(i=arr.length-1;i>=0;i--)
        {
            while(!stack.empty() && arr[i]>=stack.peek())
            {
                stack.pop();
            }
            result[i]=stack.empty()?-1:stack.peek();
            stack.push(arr[i]);
        }
    
        return result;
    } 
}
```

https://leetcode.com/problems/next-greater-element-i/

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        Stack<Integer> stack = new Stack<>();
        Map<Integer, Integer> numToMax = new HashMap<>();

        for (int i = nums2.length - 1; i >= 0; i--) {

            while (!stack.isEmpty() && stack.peek() < nums2[i]) {
                stack.pop();
            }

            if (stack.isEmpty()) {
                numToMax.put(nums2[i], -1);
            } else {
                numToMax.put(nums2[i], stack.peek());
            }
            stack.push(nums2[i]);
        }

        int[] res = new int[nums1.length];

        for (int i = 0; i < nums1.length; i++) {
            res[i] = numToMax.get(nums1[i]);
        }

        return res;
    }
}
```


**Time complexity:** O(n). The entire nums2 array (of size n) is scanned only once. Each of the stack's n elements are pushed and popped exactly once. The nums1 array is also scanned only once. All together this requires O(n+n+m) time. Since nums1 must be a subset of nums2, we know m must be less than or equal to n. Therefore, the time complexity can be simplified to O(n).

**Space complexity:** O(n). map will store n key-value pairs while stack will contain at most n elements at any given time.