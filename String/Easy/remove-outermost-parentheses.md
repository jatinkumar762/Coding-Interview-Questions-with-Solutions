https://leetcode.com/problems/remove-outermost-parentheses/description/

#### Approach-1 Using Stack

#### Approach-2 Using count of open parenthesis


```java
class Solution {
    public String removeOuterParentheses(String s) {

        //Stack<Character> stack = new Stack<>();

        int len = s.length();

        StringBuilder res = new StringBuilder();

        int open = 0;

        for (int i = 0; i < s.length(); i++) {

            if (s.charAt(i) == '(') {

                open++;
                //stack.push(s.charAt(i));

                if (open > 1) {
                    res.append("(");
                }

            } else {

                open--;

                //stack.pop();

                if (open > 0) {
                    res.append(")");
                }
            }

        }

        return res.toString();
    }
}
```

**Optimization by 2 ms**

```java
class Solution {
    public String removeOuterParentheses(String s) {

        //Stack<Character> stack = new Stack<>();

        int len = s.length();

        StringBuilder res = new StringBuilder();

        int open = 0;

        for (char ch : s.toCharArray()) {

            if (ch == '(') {

                open++;
                //stack.push(s.charAt(i));

                if (open > 1) {
                    res.append(ch);
                }

            } else {

                open--;

                //stack.pop();

                if (open > 0) {
                    res.append(ch);
                }
            }

        }

        return res.toString();

    }
}
```

---

```java
class Solution {
    public String removeOuterParentheses(String s) {

        StringBuilder result = new StringBuilder("");

        int openParenthesisCount = 0;
        for(char ch:s.toCharArray()){
            if(ch=='(' && openParenthesisCount++>0) result.append(ch); 
            if(ch==')' && openParenthesisCount-->1) result.append(ch); 
        }
        return result.toString();
    }
}
```
