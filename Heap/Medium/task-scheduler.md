https://leetcode.com/problems/task-scheduler/description/

### Priority Queue (Max-Heap) + Map

```java
class Solution {

    static class CharFreq {
        char ch;
        int freq;

        public CharFreq(char ch, int freq) {
            this.ch = ch;
            this.freq = freq;
        }

        @Override
        public String toString() {
            return ch + " : " + freq;
        }
    }

    public int leastInterval(char[] tasks, int n) {

        Map<Character, Integer> freqMap = new HashMap<>();

        for (char ch : tasks) {
            freqMap.put(ch, freqMap.getOrDefault(ch, 0) + 1);
        }

        PriorityQueue<CharFreq> pq = new PriorityQueue<CharFreq>((a, b) -> b.freq - a.freq);

        freqMap.entrySet().stream()
                .map(entry -> new CharFreq(entry.getKey(), entry.getValue()))
                .forEach(pq::add);

        int count = 0;
        while (!pq.isEmpty()) {

            int window = n + 1;

            freqMap = new HashMap<>();

            while (window > 0 && !pq.isEmpty()) {

                CharFreq task = pq.poll();
                count += 1;

                if (task.freq > 1) {
                    freqMap.put(task.ch, task.freq - 1);
                }

                window -= 1;
            }

            //idle
            if (!freqMap.isEmpty() && window > 0) {
                count += window;
            }

            freqMap.entrySet().stream()
                    .map(entry -> new CharFreq(entry.getKey(), entry.getValue()))
                    .forEach(pq::add);
        }

        return count;
    }
}
```

