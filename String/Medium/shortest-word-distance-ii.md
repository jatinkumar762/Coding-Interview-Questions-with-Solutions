https://leetcode.com/problems/shortest-word-distance-ii/description/


```java
class WordDistance {

    private Map<String, List<Integer>> wordToIndex;

    public WordDistance(String[] wordsDict) {

        wordToIndex = new HashMap<>();

        int len = wordsDict.length;

        for (int i = 0; i < len; i++) {

            wordToIndex.putIfAbsent(wordsDict[i], new ArrayList<>());

            wordToIndex.get(wordsDict[i]).add(i);
        }
    }

    public int shortest(String word1, String word2) {

        //will be in increasing order default (lower index added first)
        List<Integer> word1Index = wordToIndex.get(word1);

        List<Integer> word2Index = wordToIndex.get(word2);

        int len1 = word1Index.size();
        int len2 = word2Index.size();

        int pointer_1 = 0;
        int pointer_2 = 0;

        int min = Integer.MAX_VALUE;

        while (pointer_1 < len1 && pointer_2 < len2) {

            min = Math.min(min, Math.abs(word1Index.get(pointer_1) - word2Index.get(pointer_2)));

            if (word1Index.get(pointer_1) <= word2Index.get(pointer_2)) {
                pointer_1++;
            } else {
                pointer_2++;
            }

        }

        return min;
    }
}

/**
 * Your WordDistance object will be instantiated and called as such:
 * WordDistance obj = new WordDistance(wordsDict);
 * int param_1 = obj.shortest(word1,word2);
 */
```