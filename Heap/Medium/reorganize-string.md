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

---

## Correct Approach

1. Use Max Heap
2. First put character at even positions, then on odd positions
3. if any character freq is > $(s.length() + 1)/2$ then return ""


```
Input
s = "baaba"

Output: "abaab"

Expected: "ababa"
```

**Incorrect**

```java
char[] arr = new char[s.length()];

int index = 0;
while (!pq.isEmpty()) {

    Pair<Character, Integer> element = pq.poll();

    arr[index] = element.getKey();

    index += 2;

    if(element.getValue() > 1){
        pq.add(new Pair(element.getKey(), element.getValue()-1));
    }

    if(index >= len){
        index = 1;
    }
}
```
---

- while reinserting order can change


**Complete Solution**

```java

class Solution {
    public String reorganizeString(String s) {

        int len = s.length();

        Map<Character, Integer> map = new HashMap();

        for (char ch : s.toCharArray()) {

            map.put(ch, map.getOrDefault(ch, 0) + 1);

            if(map.get(ch) > (len+1)/2){
                return "";
            }
        }

        Comparator<Pair<Character, Integer>> comparator = new Comparator<Pair<Character, Integer>>() {

            @Override
            public int compare(Pair<Character, Integer> p1, Pair<Character, Integer> p2) {

                if (p1.getValue() < p2.getValue()) {
                    return 1;
                }

                return -1;
            }
        };

        PriorityQueue<Pair<Character, Integer>> pq = new PriorityQueue<>(comparator);

        map.forEach((key, value) -> pq.add(new Pair(key, value)));

        char[] arr = new char[s.length()];

        int index = 0;
        while (!pq.isEmpty()) {

            Pair<Character, Integer> element = pq.poll();

            for(int freq = 1; freq <= element.getValue(); freq++){

                arr[index] = element.getKey();

                index+=2;

                if(index >= len){
                    index = 1;
                }
            }
            
        }

        return new String(arr);
    }
}
```