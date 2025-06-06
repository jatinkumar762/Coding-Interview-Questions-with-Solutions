https://practice.geeksforgeeks.org/problems/longest-k-unique-characters-substring0853/1/

```java
class Solution {
    public int longestkSubstr(String s, int k) {
        // code here
        Map<Character, Integer> uniqCount = new HashMap<>();
        
        int maxLen = -1;
        int left = 0;
        
        int length = s.length();
        
        for(int right=0; right<length; right++){
            
    
            uniqCount.put(s.charAt(right), uniqCount.getOrDefault(s.charAt(right), 0) + 1);
                

            int count = 0;
            
            while(uniqCount.size() > k) {
            
               count = uniqCount.get(s.charAt(left));
               
               if(count==1) {
                   uniqCount.remove(s.charAt(left));
               }
               else{
                   uniqCount.put(s.charAt(left), count - 1);
               }
               
               left++;
            }
                
                            
            if(uniqCount.size() == k && maxLen < (right-left+1)) {
                maxLen = right-left+1;
            }   
            
        }
        
        return maxLen;
    }
}
```
