https://leetcode.com/problems/valid-parenthesis-string/description/

#### Approach 1: Using Two Stacks

```java
class Solution {
    public boolean checkValidString(String s) {
        
        Stack<Integer> openBracketIndexStack = new Stack<>();
        Stack<Integer> asterisksIndexStack = new Stack<>();

        int index = 0;
        for(Character c:s.toCharArray()){

            if(c=='('){
                openBracketIndexStack.add(index);

            } else if(c==')'){

                if(openBracketIndexStack.size()>0){
                    openBracketIndexStack.pop();
                } else if(asterisksIndexStack.size()>0){
                    asterisksIndexStack.pop();
                } else return false;

            } else if(c=='*'){
                asterisksIndexStack.add(index);
            }

            index+=1;
        }

        /*
        if(openBracketIndexStack.size()>asterisksIndexStack.size()) return false;

        while(openBracketIndexStack.size()>0){

            int openBracketIndex = openBracketIndexStack.pop();
            int asterisksIndex = asterisksIndexStack.pop();

            if(openBracketIndex>asterisksIndex) return false;
        }
        */

        while(!openBracketIndexStack.isEmpty() && !asterisksIndexStack.isEmpty()){
            if(openBracketIndexStack.pop()>asterisksIndexStack.pop()){
                return false;
            }
        }

        return openBracketIndexStack.isEmpty();
    }
}
```

#### Approach 2: Two Pointer

```java
class Solution {
    public boolean checkValidString(String s) {
        int openCount = 0;
        int closeCount = 0;
        int length = s.length() - 1;
        
        // Traverse the string from both ends simultaneously

        //traversing left side -> open brackets are able to balance closed brackets -> if count more than zero, means bcz of *
        //traversing right side -> close brackets are able to balance open brackets -> if count more than zero, means bcz of *

        for (int i = 0; i <= length; i++) {
            // Count open parentheses or asterisks
            if (s.charAt(i) == '(' || s.charAt(i) == '*') {
                openCount++;
            } else {
                openCount--;
            }
            
            // Count close parentheses or asterisks
            if (s.charAt(length - i) == ')' || s.charAt(length - i) == '*') {
                closeCount++;
            } else {
                closeCount--;
            }
            
            // If at any point open count or close count goes negative, the string is invalid
            if (openCount < 0 || closeCount < 0) {
                return false;
            }
        }
        
        // If open count and close count are both non-negative, the string is valid
        return true;
    }
}
```