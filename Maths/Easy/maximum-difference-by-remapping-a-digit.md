https://leetcode.com/problems/maximum-difference-by-remapping-a-digit/description/


### Greedy Approach

```java
class Solution {
    public int minMaxDifference(int num) {
        
        char[] input = String.valueOf(num).toCharArray();

        int len = input.length;

        int max = -1;
        for(int i = 0; i < len; i++){

            if(input[i] - 48 < 9){
                max = input[i] - 48;
                break;
            }
        }

        StringBuilder maxStr = new StringBuilder("");
        for(int i = 0; i < len; i++){
            if(input[i] - 48 == max ){
                maxStr.append(9);
            }else {
                maxStr.append(input[i]);
            }
        }

        int min = -1;
        for(int i = 0; i < len; i++){

            if(input[i] - 48 > 0){
                min = input[i] - 48;
                break;
            }
        }

    

        StringBuilder minStr = new StringBuilder("");
        for(int i = 0; i < len; i++){
            if(input[i] - 48 == min ){
                minStr.append(0);
            }else {
                minStr.append(input[i]);
            }
        }


        Integer maxNum = Integer.parseInt(maxStr.toString());

        Integer minNum = Integer.parseInt(minStr.toString());


        return maxNum - minNum;
    }
}
```

### leetcode solution

```java
class Solution {

    public int minMaxDifference(int num) {
        String s = Integer.toString(num);
        String t = s;
        int pos = 0;
        while (pos < s.length() && s.charAt(pos) == '9') {
            pos++;
        }
        if (pos < s.length()) {
            s = s.replace(s.charAt(pos), '9');
        }
        t = t.replace(t.charAt(0), '0');
        return Integer.parseInt(s) - Integer.parseInt(t);
    }
}
```