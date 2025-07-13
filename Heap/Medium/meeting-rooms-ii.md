https://leetcode.com/problems/meeting-rooms-ii/description/


### Sorting + Heap

```java
class Solution {

    static class Meeting {
        int start;
        int end;

        public Meeting(int start, int end) {
            this.start = start;
            this.end = end;
        }

        @Override
        public String toString() {
            return start + " " + end;
        }
    }

    public int minMeetingRooms(int[][] intervals) {

        List<Meeting> meetings = new ArrayList<>();

        for (int[] interval : intervals) {
            meetings.add(new Meeting(interval[0], interval[1]));
        }

        Collections.sort(meetings, (a, b) -> a.start - b.start);

        PriorityQueue<Meeting> pq = new PriorityQueue<Meeting>((a, b) -> a.end - b.end);

        pq.add(meetings.get(0));

        int maxRoom = 1;
        int len = intervals.length;
        int count = 1;

        for (int i = 1; i < len; i++) {

            while (!pq.isEmpty() && (pq.peek().end <= meetings.get(i).start)) {
                pq.poll();
                count--;
            }

            pq.add(meetings.get(i));
            count++;

            if (maxRoom < count) {
                maxRoom = count;
            }
        }

        return maxRoom;
    }
}
```

**Optimization**

- Use int[] directly,	saves object creation.

```java
class Solution {

    public int minMeetingRooms(int[][] intervals) {

        if (intervals == null || intervals.length == 0)
            return 0;

        // Sort by start time
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

        // Min-heap to track the earliest ending meeting
        PriorityQueue<Integer> pq = new PriorityQueue<Integer>();

        pq.add(intervals[0][1]);

        int maxRoom = 1;
        int len = intervals.length;
        int count = 1;

        for (int i = 1; i < len; i++) {

            while (!pq.isEmpty() && (pq.peek() <= intervals[i][0])) {
                pq.poll();
                count--;
            }

            pq.add(intervals[i][1]);
            count++;

            if (maxRoom < count) {
                maxRoom = count;
            }
        }

        return maxRoom;
    }
}
```