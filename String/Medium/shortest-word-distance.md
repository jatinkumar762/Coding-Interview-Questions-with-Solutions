https://leetcode.com/problems/shortest-word-distance/description/


### Greedy Approach

```java
class Solution {

    public int shortestDistance(String[] wordsDict, String word1, String word2) {

        int index_1 = -1;
        int index_2 = -1;

        int min = Integer.MAX_VALUE;
        int len = wordsDict.length;
        
        for (int i = 0; i < len; i++) {

            if (word1.equals(wordsDict[i])) {
                index_1 = i;
            } else if (word2.equals(wordsDict[i])) {
                index_2 = i;
            }

            if (index_1 != -1 && index_2 != -1) {
                min = Math.min(min, Math.abs(index_1 - index_2));
            }
        }

        return min;
    }
}
```

Time complexity: $O(N⋅M)$ where N is the number of words in the input list, and M is the total length of two input words.

M &rarr; for each index we are comparing string

Space complexity: O(1), since no additional space is allocated.