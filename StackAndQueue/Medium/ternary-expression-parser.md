https://leetcode.com/problems/ternary-expression-parser/description/


### Stack

```java
class Solution {
    public String parseTernary(String expression) {

        char[] arr = expression.toCharArray();

        int len = expression.length();

        Stack<Character> stack = new Stack<>();

        int i = len - 1;

        while (i >= 0) {

            char ch = arr[i];

            if (ch >= '0' && ch <= '9' || ch == 'T' || ch == 'F') {
                stack.push(ch);
            } else if (ch == '?') {
                char onTrue = stack.pop();
                char onFalse = stack.pop();

                stack.push(expression.charAt(i - 1) == 'T' ? onTrue : onFalse);

                // Decrement i by 1 as we have already used
                // Previous Boolean character
                i--;
            }

            // Go to the previous character
            i--;
        }

        return String.valueOf(stack.peek());
    }
}
```

### Recursion

```java
class Solution {
    private int i; // global index into the string

    public String parseTernary(String expression) {
        i = 0;
        return dfs(expression);
    }

    private String dfs(String s) {
        // 1) Read the “condition” or value character at s[i]
        char c = s.charAt(i++);

        // 2) If the next char is '?', we have a ternary of the form 
        //     condition ? trueExpr : falseExpr
        if (i < s.length() && s.charAt(i) == '?') {
            i++; // skip '?'

            // 3) Recursively parse the “true” branch
            String trueExpr = dfs(s);

            i++; // skip ':'

            // 4) Recursively parse the “false” branch
            String falseExpr = dfs(s);

            // 5) Choose based on the condition we read at step 1
            return c == 'T' ? trueExpr : falseExpr;
        }

        // 6) Otherwise, c itself *is* the (single-digit or letter) value
        return String.valueOf(c);
    }
}
```