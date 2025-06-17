https://leetcode.com/problems/gas-station/description/

### TLE Solution

```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {

        int len = gas.length;

        int sumGas = 0;
        int sumCost = 0;
        for (int i = 0; i < len; i++) {
            sumGas += gas[i];
            sumCost += cost[i];
        }

        if (sumGas < sumCost) {
            return -1;
        }

        for (int i = 0; i < len; i++) {

            if (gas[i] < cost[i]) {
                continue;
            }

            if (canTravel(i, gas, cost, len)) {
                return i;
            }
        }

        return -1;
    }

    private boolean canTravel(int startIndex, int[] gas, int[] cost, int len) {

        int tank = 0;

        for (int i = 0; i < len; i++) {

            int index = (startIndex + i) % len;

            tank = tank + gas[index];

            if (cost[index] > tank) {
                return false;
            }

            tank = tank - cost[index];
        }

        return true;
    }

}
```

Time Complexity: $O(N^2)$


### One Pass

```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {

        int len = gas.length;

        /*
        int sumGas = 0;
        int sumCost = 0;
        for (int i = 0; i < len; i++) {
            sumGas += gas[i];
            sumCost += cost[i];
        }
        
        if (sumGas < sumCost) {
            return -1;
        }
        */

        int total = 0;
        for (int i = 0; i < len; i++) {
            total += gas[i] - cost[i];
        }

        if (total < 0) {
            return -1;
        }

        //solution definitely exist - if we are here
        //in question given - unique solution exist

        int[] diff = new int[len];

        for (int i = 0; i < len; i++) {
            diff[i] = gas[i] - cost[i];
        }

        int startPos = 0;
        int totalSum = 0;
        for (int i = 0; i < len; i++) {

            totalSum += diff[i];

            if (totalSum < 0) {
                totalSum = 0;
                startPos = i + 1;
            }
        }

        return startPos;
    }

}
```

---

* leetcode solution 

```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int currGain = 0, totalGain = 0, answer = 0;

        for (int i = 0; i < gas.length; ++i) {
            // gain[i] = gas[i] - cost[i]
            totalGain += gas[i] - cost[i];
            currGain += gas[i] - cost[i];

            // If we meet a "valley", start over from the next station
            // with 0 initial gas.
            if (currGain < 0) {
                answer = i + 1;
                currGain = 0;
            }
        }

        return totalGain >= 0 ? answer : -1;
    }
}
```