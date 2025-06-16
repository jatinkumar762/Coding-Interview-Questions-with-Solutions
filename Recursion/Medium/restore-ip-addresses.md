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