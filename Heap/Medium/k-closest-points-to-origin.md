https://leetcode.com/problems/k-closest-points-to-origin/description/

### Sorting

```java
class Solution {
    public int[][] kClosest(int[][] points, int k) {

        //sort in ascending order
        Arrays.sort(points, (a, b) -> squaredDistance(a) - squaredDistance(b));

        int[][] results = new int[k][2];

        for (int i = 0; i < k; i++) {
            results[i] = points[i];
        }

        return results;
    }

    private int squaredDistance(int[] point) {
        // Calculate and return the squared Euclidean distance
        return point[0] * point[0] + point[1] * point[1];
    }
}
```

### MaxHeap

```java
class Point {

    int x;
    int y;
    //double dist;
    long dist;

    Point(int x, int y) {
        this.x = x;
        this.y = y;
        //this.dist = Math.sqrt(x * x + y * y);
        this.dist = x * x + y * y;
    }

    @Override
    public String toString() {
        return String.format("x: %s, y: %s, dist: %s", x, y, dist);
    }
}

class Solution {
    public int[][] kClosest(int[][] points, int k) {

        //WILL BUILD MAX HEAP of K SIZE
        //REMAINING ELEMENTS OF HEAP ARE K SMALL 

        Comparator<Point> pointComparator = new Comparator<Point>() {

            @Override
            public int compare(Point a, Point b) {

                if (a.dist < b.dist) {
                    return 1;
                }
                
                //dont return 0
                //otherwise we are telling that both elements are equal
                return -1;
            }
        };

        PriorityQueue<Point> pq = new PriorityQueue<Point>(pointComparator);

        int count = 0;
        for (int[] point : points) {
            
            pq.add(new Point(point[0], point[1]));

            count++;

            if (count > k) {
                pq.poll();
            }
        }

        int[][] result = new int[k][2];
        int index = 0;
        while (!pq.isEmpty()) {

            Point p = pq.poll();

            result[index][0] = p.x;
            result[index][1] = p.y;

            index++;
        }

        return result;
    }
}
```

### ToDo - QuickSort