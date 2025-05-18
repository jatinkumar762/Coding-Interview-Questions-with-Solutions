https://leetcode.com/problems/next-greater-element-ii/description/


* Instead of creating Double length array, we can use two loops
* Loop once, we can get the Next Greater Number of a normal array.
* Loop twice, we can get the Next Greater Number of a circular array


### Approach-1 18ms 

```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        
        int N = nums.length;

        int[] res = new int[N];
        Arrays.fill(res, -1);

        Stack<Integer> stack = new Stack<>();
        // we store index of num array in stack to avoid duplicate elements

        for(int i=2*N-1; i>=0; i--){
            
            while(!stack.isEmpty() && stack.peek() <= nums[i%N] ){
                stack.pop();
            }

            if(!stack.isEmpty()){
                //duplicate possible, so can't store against elements of nums
                //based on index storing corresponding result
                res[i%N] = stack.peek();
            }

            stack.push(nums[i%N]);
        }
        return res;
    }
}
```

### Approach-2 15ms

```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {

        int N = nums.length;

        int[] res = new int[N];
        Arrays.fill(res, -1);

        Stack<Integer> stack = new Stack<>();

        for (int i=0; i<2*N; i++) {
            while (!stack.isEmpty() && nums[stack.peek()] < nums[i % N]) {
                res[stack.pop()] = nums[i%N];
            }
            stack.push(i % N);
        }
        return res;
    }
}
```

### Leetcode solution

```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {

        int[] res = new int[nums.length];
        Stack<Integer> stack = new Stack<>();
        for (int i = 2 * nums.length - 1; i >= 0; --i) {
            while (!stack.empty() && nums[stack.peek()] <= nums[i % nums.length]) {
                stack.pop();
            }
            res[i % nums.length] = stack.empty() ? -1 : nums[stack.peek()];
            stack.push(i % nums.length);
        }
        return res;
    }
}
```