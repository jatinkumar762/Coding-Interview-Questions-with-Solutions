https://leetcode.com/problems/wildcard-matching/description/


### Recursive Solution

```java

class Solution {
    public boolean isMatch(String s, String p) {

        char[] arr1 = s.toCharArray();
        char[] arr2 = p.toCharArray();

        int len1 = s.length();
        int len2 = p.length();

        return wildcardMatching(arr1, arr2, len1 - 1, len2 - 1);
    }

    private boolean wildcardMatching(char[] arr1, char[] arr2, int i, int j) {

        if (i < 0 && j >= 0) {

            while (j >= 0 && arr2[j] == '*') {
                j--;
            }

            return j < 0;
        }

        if (i < 0 && j < 0) {
            return true;
        }

        if (i >= 0 && j < 0) {
            return false;
        }

        if (arr1[i] == arr2[j] || arr2[j] == '?') {
            return wildcardMatching(arr1, arr2, i - 1, j - 1);
        } else if (arr2[j] == '*') {

            //case-1: * matched with s char
            //case-2: * considered as empty

            return wildcardMatching(arr1, arr2, i - 1, j) || wildcardMatching(arr1, arr2, i, j - 1);
        }
        // } else if (arr1[i] != arr2[j]) {
        //     return false;
        // }

        return false;
    }
}
```