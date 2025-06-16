https://leetcode.com/problems/design-hit-counter/description/


```java
class HitCounter {

    Map<Integer, Integer> map;
    int count;

    public HitCounter() {
        map = new HashMap<>();
        count = 0;
    }

    public void hit(int timestamp) {
        count++;
        map.put(timestamp, map.getOrDefault(timestamp, 0) + 1);
    }

    public int getHits(int timestamp) {

        int sum = 0;
        for (Integer key : map.keySet()) {
            if ((timestamp - 300) >= key) {
                sum += map.getOrDefault(key, 0);
            }
        }

        return count - sum;
    }
}

/**
 * Your HitCounter object will be instantiated and called as such:
 * HitCounter obj = new HitCounter();
 * obj.hit(timestamp);
 * int param_2 = obj.getHits(timestamp);
 */
```

### Using Queue (leetcode solution)

```java
class HitCounter {
    private Queue<Integer> hits; 

    /** Initialize your data structure here. */
    public HitCounter() {
        this.hits = new LinkedList<Integer>();
    }
    
    /** Record a hit.
        @param timestamp - The current timestamp (in seconds granularity). */
    public void hit(int timestamp) {
        this.hits.add(timestamp);
    }
    
    /** Return the number of hits in the past 5 minutes.
        @param timestamp - The current timestamp (in seconds granularity). */
    public int getHits(int timestamp) {
        while (!this.hits.isEmpty()) {
            int diff = timestamp - this.hits.peek();
            if (diff >= 300) this.hits.remove();
            else break;
        }
        return this.hits.size();
    }
}
```
