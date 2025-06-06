* [Longest Prefix Suffix](https://practice.geeksforgeeks.org/problems/longest-prefix-suffix2527/1)
* [Longest Happy Prefix](https://leetcode.com/problems/longest-happy-prefix/)

Pattern - a a b a a b a a a

Prefix/Suffix Array - [0, 1, 0, 1, 2, 3, 4, 5, 2]

```java
/**
* Compute temporary array to maintain size of suffix which is same as prefix
* Time/space complexity is O(size of pattern)
*/
class Solution {
    public String longestPrefix(String s) {
        
        if(s.length()==1)
            return "";
        
        // code here
        int len = s.length();
        
        int[] count = new int[s.length()];
        
        count[0] = 0;

        for(int i=0, j=i+1; j<len; ){
            
            if(s.charAt(i) == s.charAt(j)){
                
                count[j] = i+1;
                j++; i++;
            
            } else {

                if(i!=0){
                    i = count[i-1];
                } else {
                    count[j]=0;
                    j++;
                }
            } 
        }
        
        return s.substring(0,count[len-1]);
    }
}
```

#### KMP Pattern search code implementation

```java
import java.util.*;
import java.lang.*;
import java.io.*;

class Codechef
{
    private static int[] lps(String s) {
        // code here
        int len = s.length();
        
        int[] count = new int[s.length()];
        
        count[0] = 0;
        if(s.length()==1)
            return count;
            
        for(int i=0,j=i+1;j<len;){
            if(s.charAt(i)==s.charAt(j)){
                count[j] = i+1;
                j++; i++;
            } else {
                if(i!=0){
                    i = count[i-1];
                } else {
                    count[j]=0;
                    j++;
                }
            }
            
        }
        return count;
    }
    
    private static boolean KMP(String text, String pattern){
        int[] count = lps(pattern);
        int i=0,j=0;
        while(i<text.length() && j<pattern.length()){
            
            if(text.charAt(i)==pattern.charAt(j)){
                i++;j++;
            } else {
                if(j!=0){
                    j = count[j-1];
                } else {
                    i++;
                }
            }
        }
        if(j==pattern.length())
          return true;
        return false;
    }
    
	public static void main (String[] args) throws java.lang.Exception
	{
		// your code goes here
		String text = "THIS IS A TEST TEXT";
		String pattern = "TEST";
		
		boolean result = KMP(text, pattern);
		System.out.println(result);
	}
}
```


---

* https://www.youtube.com/watch?v=GTJr8OvyEVQ&ab_channel=TusharRoy-CodingMadeSimple


The **Knuth-Morris-Pratt (KMP)** algorithm is used for **pattern matching** in strings. It is efficient for finding all occurrences of a pattern within a text in \(O(n + m)\), where \(n\) is the length of the text and \(m\) is the length of the pattern. The efficiency comes from precomputing a **Longest Prefix Suffix (LPS)** array that avoids redundant comparisons.

---

### Steps in KMP Algorithm:

1. **Build the LPS Array:**
   - The LPS array indicates the length of the longest prefix of the pattern that is also a suffix for every position in the pattern. 
   - This helps to avoid re-checking characters in the text and pattern.

2. **Pattern Matching:**
   - Compare the pattern with the text using the LPS array to skip unnecessary comparisons when a mismatch occurs.

---

### Example:

#### Input:
- **Text:** `"ababcababcabc"`
- **Pattern:** `"ababc"`

---

### Step 1: Build the LPS Array

| Index | Pattern | LPS Value | Explanation                          |
|-------|---------|-----------|--------------------------------------|
| 0     | `a`     | 0         | No proper prefix and suffix.         |
| 1     | `ab`    | 0         | No proper prefix and suffix.         |
| 2     | `aba`   | 1         | `a` is both a prefix and suffix.     |
| 3     | `abab`  | 2         | `ab` is both a prefix and suffix.    |
| 4     | `ababc` | 0         | No proper prefix and suffix.         |

LPS Array: `[0, 0, 1, 2, 0]`

---

### Step 2: Pattern Matching

- Start comparing the pattern with the text.
- Use the LPS array to skip unnecessary comparisons when a mismatch occurs.

| Text Index | Text   | Pattern Comparison | LPS Used | Explanation                     |
|------------|--------|---------------------|----------|---------------------------------|
| 0          | `ababc`| Match               | -        | Characters match.               |
| 5          | `ababc`| Match               | -        | Pattern found at index 0.       |
| 6          | `ababc`| Match               | LPS = 2  | Skip unnecessary checks.        |
| 11         | `ababc`| Match               | LPS = 2  | Pattern found at index 6.       |

Result: Pattern is found at indices `0` and `6`.

---

https://leetcode.com/problems/rotate-string/description/


* solve using kmp, if contains function not allowed

```java
class Solution {

    public boolean rotateString(String s, String goal) {

        if(s.length()!=goal.length()){
            return false;
        }

        String ss = s + s;

        int[] prefixSuffixArray = prefixSuffixArray(goal);
        
        //System.out.println(Arrays.toString(prefixSuffixArray));

        return checkSubStringPresent(ss, goal, prefixSuffixArray);
    }

    private boolean checkSubStringPresent(String str, String subStr, int[] prefixSuffixArray) {

        int strLen = str.length();
        int subStrLen = subStr.length();

        for(int i=0, j=0; i < strLen; ){

            if(str.charAt(i) == subStr.charAt(j)){
                j++;
                i++;
            } else {

                if(j!=0){

                    //why j-1, bcz till j-1 and i-1, already matching
                    j = prefixSuffixArray[j-1];
                } else {
                    i++;
                }

            }

            if(j == subStrLen) {
                return true;
            }
        }

        return false;
    }

    private int[] prefixSuffixArray(String pattern) {

        int len = pattern.length();

        int[] prefixSuffixArray = new int[len];

        prefixSuffixArray[0] = 0; //overlapping but not self

        for (int left = 0, right = 1; right < len;) {

            if (pattern.charAt(left) == pattern.charAt(right)) {
                prefixSuffixArray[right] = left + 1; //length
                left++;
                right++;
            } else {

                if (left != 0) {
                    left = prefixSuffixArray[left-1];
                } else {
                    prefixSuffixArray[right] = 0;
                    right++;
                }
            }
        }

        return prefixSuffixArray;
    }
}
```



---

### Implementation in Java

```java
public class KMPAlgorithm {

    /**
     * Computes the Longest Prefix Suffix (LPS) array for the given pattern.
     * The LPS array is used to skip unnecessary comparisons in the KMP algorithm.
     * 
     * @param pattern The pattern for which to compute the LPS array.
     * @return The LPS array.
     */
    public static int[] computeLPS(String pattern) {
        int patternLen = pattern.length(); // m
        int[] lps = new int[patternLen];
        
        // Edge case: if the pattern is empty, return an empty array
        if (patternLen == 0) {
            return lps;
        }

        int j = 0; // Length of previous longest prefix suffix
        int i = 1; // Start comparing from the second character

        while (i < patternLen) {
            // If there's a match
            if (pattern.charAt(i) == pattern.charAt(j)) {
                j++; // Increment both pointers
                lps[i] = j; // Store length of the matched prefix-suffix
                i++; // Move to the next character in the pattern
            } else {
                // If mismatch occurs and j > 0, try the previously found shorter prefix
                if (j > 0) {
                    j = lps[j - 1]; // Move to the previous longest prefix-suffix
                } else {
                    lps[i] = 0; // No prefix-suffix found
                    i++; // Move to the next character
                }
            }
        }
        return lps;
    }


    /**
     * Perform KMP search to find all occurrences of the pattern in the text.
     * @param text The text in which to search for the pattern.
     * @param pattern The pattern to search for in the text.
     */
    public static void KMPSearch(String text, String pattern) {
        int n = text.length();
        int m = pattern.length();
        
        // Compute the LPS array for the pattern
        int[] lps = computeLPS(pattern);

        int i = 0; // Index for text
        int j = 0; // Index for pattern

        while (i < n) {
            // Match current characters of text and pattern
            if (pattern.charAt(j) == text.charAt(i)) {
                i++;
                j++;
            }

            // If we found a match of the whole pattern
            if (j == m) {
                System.out.println("Pattern found at index " + (i - j));
                // Use LPS to avoid unnecessary comparisons
                j = lps[j - 1];
            }
            // Mismatch handling: use LPS to skip characters in the pattern
            else if (i < n && pattern.charAt(j) != text.charAt(i)) {
                if (j > 0) {
                    // Use LPS to shift the pattern position
                    j = lps[j - 1];
                } else {
                    // Move to the next character in the text
                    i++;
                }
            }
        }
    }

    public static void main(String[] args) {
        String text = "ababcababcabc";   //aaaa
        String pattern = "ababc";       //aa

        KMPSearch(text, pattern);
    }
}
```

---

### Output:

```
Pattern found at index 0
Pattern found at index 6
```

---

### Key Points:
1. **LPS Array:** The key to the efficiency of KMP.
2. **Avoid Redundant Comparisons:** Use the LPS array to skip unnecessary character checks.
3. **Time Complexity:** 
   - Precomputing LPS: $O(m)$
   - Searching: $O(n)$
   - Total: $O(n + m)$