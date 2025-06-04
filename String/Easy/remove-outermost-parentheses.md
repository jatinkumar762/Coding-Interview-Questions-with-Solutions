https://leetcode.com/problems/remove-outermost-parentheses/description/

#### Approach-1 Using Stack

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

#### Approach-2 Using count of open parenthesis