https://www.geeksforgeeks.org/problems/decode-the-pattern1138/1

```java

class Solution {
    public String countAndSay(int n) {
        // code here
        
        String res = new String("");
        
        while(n > 0){
            
            res = getNextStr(res);
            
            n--;
        }
        
        return res;
    }
    
    private String getNextStr(String res){
        
        
        if(res.length() == 0){
            return "1";
        }
        
        StringBuilder newRes = new StringBuilder("");
        
        char curr = ' ';
        int count = 0;
        for(int i = 0; i < res.length(); i++){
            
            if(i == 0){
                curr = res.charAt(i);
                count++;
            } else if(curr == res.charAt(i)){
                count++;
            } else {
                
                newRes.append(count);
                newRes.append(curr);
                
                curr = res.charAt(i);
                count = 1;
            }
            
        }
        
        newRes.append(count);
        newRes.append(curr);
        
        return newRes.toString();
    }
}


```