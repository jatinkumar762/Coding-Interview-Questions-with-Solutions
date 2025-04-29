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

### Top-Down

**Note: Test Case**

s = ""
p = "****"

if we dp[len1-1][len2-1], index -1 out of bound exception can come

return dp[len1-1][len2-1] == 1

```java
class Solution {
    public boolean isMatch(String s, String p) {

        char[] arr1 = s.toCharArray();
        char[] arr2 = p.toCharArray();

        int len1 = s.length();
        int len2 = p.length();

        int[][] dp = new int[len1][len2];

        for (int[] row : dp) {
            Arrays.fill(row, -1);
        }

        return wildcardMatching(arr1, arr2, len1 - 1, len2 - 1, dp) == 1;
    }

    private int wildcardMatching(char[] arr1, char[] arr2, int i, int j, int[][] dp) {

        if (i < 0 && j >= 0) {

            while (j >= 0 && arr2[j] == '*') {
                j--;
            }

            return j < 0 ? 1 : 0;
        }

        if (i < 0 && j < 0) {
            return 1;
        }

        if (i >= 0 && j < 0) {
            return 0;
        }

        if (dp[i][j] != -1) {
            return dp[i][j];
        }

        if (arr1[i] == arr2[j] || arr2[j] == '?') {

            return dp[i][j] = wildcardMatching(arr1, arr2, i - 1, j - 1, dp);

        } else if (arr2[j] == '*') {

            //case-1: * matched with s char
            //case-2: * considered as empty
            int case1 = wildcardMatching(arr1, arr2, i - 1, j, dp);

            if (case1 == 1) {
                return dp[i][j] = 1;
            }

            int case2 = wildcardMatching(arr1, arr2, i, j - 1, dp);

            return dp[i][j] = case2;
        }
        // } else if (arr1[i] != arr2[j]) {
        //     return false;
        // }

        return dp[i][j] = 0;
    }
}
```

### Bottom-Up 


s =
"aab"

p =
"c*a*b"

Output: false

Expected: false

* s = "aab" → treated as ["", "a", "a", "b"]

* p = "c*a*b" → treated as ["", "c", "*", "a", "*", "b"]



|       s \ p       |  ""  |  c  |  *  |  a  |  *  |  b  |
|------------------|------|-----|-----|-----|-----|-----|
| **""** (s[0])     |      |     |     |     |     |     |
| **"a"** (s[1])    |      |     |     |     |     |     |
| **"a"** (s[2])    |      |     |     |     |     |     |
| **"b"** (s[3])    |      |     |     |     |     |     |


</br>

|      s \ p     | "" | c | * | a | * | b |
|----------------|----|---|---|---|---|---|
| **""**         | T  | F | F | F | F | F |
| **"a"**        | F  | F | F | F | F | F |
| **"aa"**       | F  | F | F | F | F | F |
| **"aab"**      | F  | F | F | F | F | F |


```java
class Solution {
    public boolean isMatch(String s, String p) {

        char[] arr1 = s.toCharArray();
        char[] arr2 = p.toCharArray();

        int len1 = s.length();
        int len2 = p.length();

        //default false
        boolean[][] dp = new boolean[len1 + 1][len2 + 1];
        dp[0][0] = true;

        for (int i = 1; i <= len1; i++) {
            dp[i][0] = false;
        }

        for (int j = 1; j <= len2; j++) {

            if (arr2[j - 1] == '*') {
                dp[0][j] = dp[0][j-1];
            } else {
                dp[0][j] = false;
            }

        }

        for (int i = 1; i <= len1; i++) {
            for (int j = 1; j <= len2; j++) {

                if (arr1[i - 1] == arr2[j - 1] || arr2[j - 1] == '?') {
                    dp[i][j] = dp[i - 1][j - 1];
                } else if (arr2[j - 1] == '*') {
                    dp[i][j] = dp[i - 1][j] || dp[i][j - 1];
                } else {
                    dp[i][j] = false;
                }
            }
        }

        return dp[len1][len2];
    }
}
```