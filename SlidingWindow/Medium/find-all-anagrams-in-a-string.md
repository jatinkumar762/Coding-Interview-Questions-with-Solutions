https://leetcode.com/problems/find-all-anagrams-in-a-string/description/

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {

        List<Integer> result = new ArrayList<>();

        int patternLength = p.length();
        int strLength = s.length();

        if (patternLength > strLength) {
            return result;
        }

        int[] pattFreq = getFreq(p, 0, patternLength - 1);

        int[] strFreq = getFreq(s, 0, patternLength - 1);

        if (isEqual(strFreq, pattFreq)) {
            result.add(0);
        }

        for (int index = patternLength; index < strLength; index++) {

            strFreq[s.charAt(index - patternLength) - 'a'] -= 1;
            strFreq[s.charAt(index) - 'a'] += 1;

            if (isEqual(strFreq, pattFreq)) {
                result.add(index - patternLength + 1);
            }

        }

        return result;
    }

    private int[] getFreq(String str, int start, int end) {

        int[] freq = new int[26];

        for (int i = start; i <= end; i++) {
            freq[str.charAt(i) - 'a']++;
        }

        return freq;
    }

    private boolean isEqual(int[] a, int[] b) {

        for (int i = 0; i < 26; i++) {
            if (a[i] != b[i]) {
                return false;
            }
        }

        return true;
    }
}
```