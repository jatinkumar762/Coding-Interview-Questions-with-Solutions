https://leetcode.com/problems/word-pattern/description/

* similar to problem https://leetcode.com/problems/isomorphic-strings/description/

### Approach-1 Using 2 HashMap

```java
class Solution {
    public boolean wordPattern(String pattern, String s) {

        String[] words = s.split(" ");

        char[] letters = pattern.toCharArray();

        if (pattern.length() != words.length) {
            return false;
        }

        Map<Character, String> letterToWord = new HashMap<>();
        Map<String, Character> WordToLetter = new HashMap<>();

        for (int i = 0; i < pattern.length(); i++) {

            if (letterToWord.containsKey(letters[i])) {
                if (!letterToWord.get(letters[i]).equals(words[i])) {
                    return false;
                }
            } else {
                letterToWord.put(letters[i], words[i]);
            }

            if (WordToLetter.containsKey(words[i])) {
                if (WordToLetter.get(words[i]) != letters[i]) {
                    return false;
                }
            } else {
                WordToLetter.put(words[i], letters[i]);
            }
        }

        return true;
    }
}
```

### Approach-2 Using Single Map

**Special Test Case:**

pattern = "abc"
s = "b c a"

Output: True
Expected: True

* below solution will give False for above test case
* we can append '$' in string from word, so it will become different

```java
class Solution {
    public boolean wordPattern(String pattern, String s) {

        HashMap<String, Integer> map_index = new HashMap<>();
        String[] words = s.split(" ");

        // If the number of words is different from the length of the pattern, return false
        if (words.length != pattern.length()) 
            return false;

        for (Integer i = 0; i < words.length; i++) {
            char c = pattern.charAt(i);
            //String w = words[i];
            String w = words[i] + "$";

            // Check if the character c has been seen before, if not, add it to the map with its index
            if (!map_index.containsKey(String.valueOf(c))) 
                map_index.put(String.valueOf(c), i);

            // Check if the word w has been seen before, if not, add it to the map with its index
            if (!map_index.containsKey(w)) 
                map_index.put(w, i);

            // Ensure that the character c and word w map to the same index
            if (!map_index.get(String.valueOf(c)).equals(map_index.get(w))) 
                return false; // Return false if there's a mismatch
        }

        return true;
    }
}
```

**Note:**

* we can use Map<String, String> also
* we store map.put(String.valueOf(ch), word)
* we store map.put(word, String.valueOf(ch))