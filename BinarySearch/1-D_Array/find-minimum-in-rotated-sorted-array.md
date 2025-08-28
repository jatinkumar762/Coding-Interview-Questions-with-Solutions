https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/

```java
class Solution {
    public int findMin(int[] arr) {
        
        // complete the function here
        int left = 0;
        int right = arr.length - 1;
        int mid = left + (right-left)/2;
        
        while(left<=right){
            
            // The current subarray is already sorted,
            // the minimum is at the low index
            if(arr[left]<=arr[right]){
                return arr[left];
            }
            
            mid = left + (right-left)/2;
            
            if(arr[mid]>arr[right]){
                // The right half is not sorted. So
                // the minimum element must be in the
                // right half.
                left = mid +1;
            } else {
                
                // The right half is sorted. Note that in
                // this case, we do not change high to mid - 1
                // but keep it to mid. The mid element
                // itself can be the smallest
                right = mid;
            }
        }
        
         return arr[mid]; //arr[left] will also work
    }
}
```