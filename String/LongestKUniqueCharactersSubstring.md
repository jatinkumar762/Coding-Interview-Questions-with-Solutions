https://practice.geeksforgeeks.org/problems/longest-k-unique-characters-substring0853/1/

```java
class Solution {
    public int longestkSubstr(String s, int k) {
        // code here
        
        Map<Character, Integer> uniqCount = new HashMap<Character, Integer>();
        
        int maxLen = -1;
        int start = 0;
        
        for(int i=0;i<s.length();i++){
            
            int count = 0;
            if(uniqCount.size()>0 && uniqCount.containsKey(s.charAt(i)))
                count = uniqCount.get(s.charAt(i));
            
            uniqCount.put(s.charAt(i), count+1);
                

            count = 0;
            while(uniqCount.size()>k){
            
               count = uniqCount.get(s.charAt(start));
               
               if(count==1){
                   uniqCount.remove(s.charAt(start));
               }
               else{
                   uniqCount.put(s.charAt(start), count-1);
               }
               start++;
            }
                
                            
            if(uniqCount.size() == k && maxLen<(i-start+1) ){
                maxLen = i-start+1;
            }   
            
        }
        
        return maxLen;
    }
}
```
