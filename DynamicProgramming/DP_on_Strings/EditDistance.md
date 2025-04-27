https://leetcode.com/problems/edit-distance/description/

f(n-1, m-1) &rarr; min operations to convert S1[0..i] to S2[0..j]

### Recursion

1. express in terms of index
2. explore all paths of matching
      1. if S1[i]==S2[j] return 0 + f(i-1, j-1)
      2. 1 + f(i, j-1) //insert
      3. 1 + f(i-1, j) //delete
      4. 1 + f(i-1, j-1) //replace 
3. return min of all paths
4. Base case
      1. s1 gets exhausted i<0
      2. s2 gets exhausted j<0

### Approach-1 Recursive Solution

```java
class Solution {
    public int minDistance(String word1, String word2) {

        char[] arr1 = word1.toCharArray();
        char[] arr2 = word2.toCharArray();

        int len1 = word1.length();
        int len2 = word2.length();

        return calMin(arr1, arr2, len1 - 1, len2 - 1);
    }

    private int calMin(char[] arr1, char[] arr2, int index1, int index2) {

        if (index1 == 0 && index2 == 0) {

            if (arr1[index1] != arr2[index2]) {
                return 1;
            } else {
                return 0;
            }
        }

        if (index1 < 0) {
            return index2 >= 0 ? index2 + 1 : 0;
        }

        if (index2 < 0) {
            return index1 >= 0 ? index1 + 1 : 0;
        }

        if (arr1[index1] == arr2[index2]) {
            
            return calMin(arr1, arr2, index1 - 1, index2 - 1);

        } else {

            int insert = 1 + calMin(arr1, arr2, index1, index2 - 1);
            int delete = 1 + calMin(arr1, arr2, index1 - 1, index2);
            int replace = 1 + calMin(arr1, arr2, index1 - 1, index2 - 1);

            return Math.min(insert, Math.min(delete, replace));
        }
    }
}
```

Time Complexity: O(3^max(m, n))

This is because:
- At each step, 3 recursive calls are made (Insert, Delete, Replace)
- The depth of the recursion is roughly m + n

