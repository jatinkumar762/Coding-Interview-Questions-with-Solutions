https://leetcode.com/problems/minimum-window-substring/description/

### Optimized Code

```java
class Solution {
    public String minWindow(String s, String t) {

        Map<Character, Integer> tMap = new HashMap<>();

        int tLen = t.length();

        for (int i = 0; i < tLen; i++) {
            tMap.put(t.charAt(i), tMap.getOrDefault(t.charAt(i), 0) + 1);
        }

        int need = tMap.size();
        int have = 0;

        int left = 0;
        int right = 0;

        int sLen = s.length();

        Map<Character, Integer> sMap = new HashMap<>();

        int[] answer = { -1, 0, 0 };

        for (; right < sLen; right++) {

            if (tMap.containsKey(s.charAt(right))) {

                sMap.put(s.charAt(right), sMap.getOrDefault(s.charAt(right), 0) + 1);

                if (tMap.get(s.charAt(right)).intValue() == sMap.get(s.charAt(right)).intValue()) {
                    have++;
                }

            }

            while (have == need && left <= right) {

                if (answer[0] == -1 || answer[0] > (right - left + 1)) {
                    answer[0] = right - left + 1;
                    answer[1] = left;
                    answer[2] = right;
                }

                if (sMap.getOrDefault(s.charAt(left), 0) >= 1) {

                    int freq = sMap.get(s.charAt(left));

                    if (freq == 1) {
                        sMap.remove(s.charAt(left));
                    } else {
                        sMap.put(s.charAt(left), freq - 1);
                    }

                    if((freq - 1) < tMap.get(s.charAt(left))){
                        have--;
                    }
                }

                left++;
            }
        }

        return answer[0] == -1 ? "" : s.substring(answer[1], answer[2] + 1);
    }
}
```

### Slower Approach

```java
class Solution {
    public String minWindow(String s, String t) {

        Map<Character, Integer> tMap = new HashMap<>();

        int tLen = t.length();

        for (int i = 0; i < tLen; i++) {
            tMap.put(t.charAt(i), tMap.getOrDefault(t.charAt(i), 0) + 1);
        }

        int left = 0;
        int right = 0;

        int sLen = s.length();

        Map<Character, Integer> sMap = new HashMap<>();

        int[] answer = { -1, 0, 0 };

        for (; right < sLen; right++) {

            if (tMap.containsKey(s.charAt(right))) {
                sMap.put(s.charAt(right), sMap.getOrDefault(s.charAt(right), 0) + 1);
            }

            while (tMap.size() == sMap.size() && compare(tMap, sMap) && left <= right) {

                //String subStr = s.substring(left, right + 1);

                if (answer[0] == -1 || answer[0] > (right - left + 1)) {
                    answer[0] = right - left + 1;
                    answer[1] = left;
                    answer[2] = right;
                }

                if (sMap.getOrDefault(s.charAt(left), 0) >= 1) {

                    int freq = sMap.get(s.charAt(left));

                    if (freq == 1) {
                        sMap.remove(s.charAt(left));
                    } else {
                        sMap.put(s.charAt(left), freq - 1);
                    }
                }

                left++;
            }
        }

        return answer[0] == -1 ? "" : s.substring(answer[1], answer[2] + 1);
    }

    private boolean compare(Map<Character, Integer> tMap, Map<Character, Integer> sMap) {

        for (Map.Entry<Character, Integer> entry : sMap.entrySet()) {

            if (tMap.get(entry.getKey()) > entry.getValue()) {
                return false;
            }
        }

        return true;
    }
}
```