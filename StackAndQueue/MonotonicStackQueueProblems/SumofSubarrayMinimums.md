https://leetcode.com/problems/sum-of-subarray-minimums/description/

### Approach-1 Brute Force 

```java
class Solution {

    int mod = 1000000007;

    public int sumSubarrayMins(int[] arr) {
        
        int sum = 0;

        for(int i = 0; i < arr.length; i++){
            int currMin = arr[i];
            for(int j = i + 1; j<arr.length; j++){
                currMin = currMin < arr[j] ? currMin : arr[j];
                sum = (sum + currMin) % mod;
            }
            sum = (sum + arr[i]) % mod; 
        }

        return sum;
    }
}
```

### Approach-2 Previous/Next Smaller Position

* this approach based on, for a array element we find its previous and next Smaller element Position
* between these position, this current array element will be minimum and contribute to sum result
<br/>

take example - 3 1 2 4

All subarray and their corresponding minimum

3           &rarr; 3\
3 1         &rarr; 1\
3 1 2       &rarr; 1\
3 1 2 4     &rarr; 1

1           &rarr; 1\
1 2         &rarr; 1\
1 2 4       &rarr; 1

2           &rarr; 2\
2 4         &rarr; 2

4           &rarr; 4


consider element 2, which is at index-2 &rarr; will check for which subarray 2 will be minimum element

now for array element 2, previous Smaller element index &rarr; 1

and next Smaller element is not present, so index &rarr; len (4)

left subarray in which 2 will contribute &rarr; {} Empty

right subarray in which 2 will contribute &rarr; (4 - index of 2) = 2 &rarr; in 2 subarray will be minimum

2\
2 4

total &rarr; (1 * 2) * 2 = 4 &larr; arr element 2 contribution


similarly we if check for element 1

left subarray which contains 1 are (1 - (-1)) = 2 &larr; -1 bcz no previous less element present

3\
{}

right subarray which contains 1 are (4 - 1) = 3 &larr; 4 bcz no next less element present

1\
1 2\
1 2 4

total &rarr; 2 * 3 = 6, see below how left combined with right

3 1\
3 1 2\
3 1 2 4

and when combine empty,

1\
1 2\
1 2 4

total = 6

element 1 contribution in sum = 6 * 1 = 6

---

**NOTE:** special case need to handle, when duplicate present

* to handle this, we can consider left/right side duplicate is smaller
* in below code considered left is smaller

arr = [71,55,82,55]

71\
71 55\
71 55 82\
71 55 82 55

55\
55 82\
55 82 55

82\
82 55

55


```java
class Solution {

    int mod = 1000000007;

    public int sumSubarrayMins(int[] arr) {

        int len = arr.length;

        int[] prevSmallIndex = new int[len];

        int[] nextSmallIndex = new int[len];

        findPrevSmall(arr, prevSmallIndex, len);

        findNextSmall(arr, nextSmallIndex, len);

        long sum = 0;

        for (int i = 0; i < len; i++) {

            sum = (sum + (long)(i - prevSmallIndex[i]) * (nextSmallIndex[i] - i) * arr[i]) % mod;

        }

        return (int)sum;
    }

    void findPrevSmall(int[] arr, int[] prevSmallIndex, int N) {

        Stack<Integer> stack = new Stack<Integer>();

        for (int i = 0; i < N; i++) {

            while (!stack.isEmpty() && arr[stack.peek()] > arr[i]) {
                stack.pop();
            }

            int index = stack.isEmpty() ? -1 : stack.peek();

            prevSmallIndex[i] = index;

            stack.push(i);
        }
    }

    void findNextSmall(int[] arr, int[] nextSmallIndex, int N) {

        Stack<Integer> stack = new Stack<Integer>();

        for (int i = N - 1; i >= 0; i--) {

            while (!stack.isEmpty() && arr[stack.peek()] >= arr[i]) {
                stack.pop();
            }

            int index = stack.isEmpty() ? N : stack.peek();

            nextSmallIndex[i] = index;

            stack.push(i);
        }
    }
}
```