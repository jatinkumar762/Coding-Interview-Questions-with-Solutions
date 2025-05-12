https://leetcode.com/problems/reorganize-string/description/


## Wrong Approach

1. count freq of each character (sort it in descending order)
2. then put character one by one until heap becomes empty 

**will fail on below test case**

s = "vvvlo"

output -> vlovv -> ""

expected -> "vlvov"



```java

/*
s = "vvvlo"

output -> vlovv -> ""

expected -> "vlvov"

*/


class Solution {
    public String reorganizeString(String s) {

        Map<Character, Integer> map = new HashMap();

        for (char ch : s.toCharArray()) {
            map.put(ch, map.getOrDefault(ch, 0) + 1);
        }

        StringBuilder res = new StringBuilder("");

        while (!map.isEmpty()) {
            
            Iterator<Map.Entry<Character, Integer>> iterator = map.entrySet().iterator();

            while(iterator.hasNext()) {
                
                Map.Entry<Character, Integer> entry = iterator.next();

                res.append(entry.getKey());

                if (entry.getValue() == 1) {
                    iterator.remove();
                } else {
                    entry.setValue(entry.getValue() - 1);
                }
            }
        }

        System.out.println(res.toString());

        for (int i = 1; i < s.length(); i++) {
            if (res.charAt(i) == res.charAt(i - 1)) {
                return "";
            }
        }

        return res.toString();
    }
}
```


## Correct Approach

1. Use Max Heap
2. First put character at even positions, then on odd positions
3. if any character freq is > $(s.length() + 1)/2$ then return ""