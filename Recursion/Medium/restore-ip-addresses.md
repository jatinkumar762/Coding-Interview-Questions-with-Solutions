https://leetcode.com/problems/restore-ip-addresses/description/

### Recursion

```java
class Solution {

    List<String> result;

    public List<String> restoreIpAddresses(String s) {

        result = new ArrayList<>();

        int len = s.length();

        if (len > 12) {
            return result;
        }

        getIpAddress(0, 0, len, s, "");

        return result;
    }

    private void getIpAddress(int i, int dots, int len, String s, String tmp) {

        if (dots == 3) {

            if (len - i > 3) {
                return;
            }

            String subStr = s.substring(i, len);

            if (checkValidSubStr(subStr)) {
                result.add(tmp + subStr);
            }

            return;
        }

        for (int j = i + 1; j < len && j <= (i + 3); j++) {

            String subStr = s.substring(i, j);

            if (checkValidSubStr(subStr)) {
                getIpAddress(j, dots + 1, len, s, tmp + subStr + ".");
            }
        }
    }

    private boolean checkValidSubStr(String subStr) {

        int num = Integer.parseInt(subStr);

        if (num == 0) {
            //"00"
            return subStr.length() == 1;
        }

        //"010"
        return num < 256 && subStr.charAt(0) != '0';
    }
}
```

Time Complexity: $O(3^N)$ where n is length of string


### Iterative - (leetcode solution)

We can make the ranges of len1, len2, len3 tighter:

len1 should be in the range [max(1, s.length() - 9), min(3, s.length() - 3] since we need to separate 3 more integers after it and the length of each integer is in [1..3].

Similarly, len2 should be in the range [max(1, s.length() - len1 - 6, min(3, s.length() - len1 - 2]

len3 should be in the range [max(1, s.length() - len1 - len2 - 3), min(3, s.length() - len1 - len2 - 1]

```java
class Solution {
    private boolean isValid(String s, int start, int length) {
        return (length == 1 ||
                (s.charAt(start) != '0' &&
                        (length < 3 ||
                                s.substring(start, start + length).compareTo("255") <= 0)));
    }

    public List<String> restoreIpAddresses(String s) {
        List<String> ans = new ArrayList<>();
        for (int len1 = Math.max(1, s.length() - 9); len1 <= 3 && len1 <= s.length() - 3; ++len1) {
            if (!isValid(s, 0, len1)) {
                continue;
            }

            for (int len2 = Math.max(1, s.length() - len1 - 6); len2 <= 3 && len2 <= s.length() - len1 - 2; ++len2) {
                if (!isValid(s, len1, len2)) {
                    continue;
                }
                for (int len3 = Math.max(1, s.length() - len1 - len2 - 3); len3 <= 3
                        && len3 <= s.length() - len1 - len2 - 1; ++len3) {
                    if (isValid(s, len1 + len2, len3) &&
                            isValid(
                                    s,
                                    len1 + len2 + len3,
                                    s.length() - len1 - len2 - len3)) {
                        ans.add(
                                String.join(
                                        ".",
                                        s.substring(0, len1),
                                        s.substring(len1, len1 + len2),
                                        s.substring(len1 + len2, len1 + len2 + len3),
                                        s.substring(len1 + len2 + len3)));
                    }
                }
            }
        }
        return ans;
    }
}
```