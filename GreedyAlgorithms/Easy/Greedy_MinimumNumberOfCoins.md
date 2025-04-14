https://www.geeksforgeeks.org/problems/-minimum-number-of-coins4426/1

Given an infinite supply of each denomination of Indian currency { 1, 2, 5, 10, 20, 50, 100, 200, 500, 2000 } and a target value N.

* each denomination of Indian currency given, so we can easily use greedy approach, otherwise greedy will not work
* suppose coins are {4, 3} and sum = 6, in this case greedy will not work
* here, each indian currency given, we can use greedy

```java
class Solution{
    
    static List<Integer> minPartition(int N)
    {
        List<Integer> result = new ArrayList<Integer>();
        // code here
        int[] currency = new int[]{ 1, 2, 5, 10, 20, 50, 100, 200, 500, 2000};
        
        int index = currency.length-1;
        
        while(N>0 && index>=0){
            while(N>=currency[index]){
                N-=currency[index];
                result.add(currency[index]);
            }
            index-=1;
        }
        
        return result;
    }
}
```

#### if they asked count of coins

count += N/currency[index]
N = N%currency[index]


## Recursive Solution

```java
class Solution {
    public int coinChange(int[] coins, int amount) {

        // min no of coins
        if (amount == 0) {
            return 0;
        }

        int n = coins.length;

        int res = calMinCoins(coins, n - 1, amount);

        if (res != Integer.MAX_VALUE) {
            return res;
        }
        return -1;
    }

    private int calMinCoins(int[] coins, int index, int amount) {

        if (amount == 0) {
            // no coins required
            return 0;
        }

        if (index == 0) {
            if (amount % coins[index] == 0) {
                return amount / coins[index];
            }
            return Integer.MAX_VALUE; // no overflow so -100
        }

        int notTake = calMinCoins(coins, index - 1, amount);

        int take = Integer.MAX_VALUE;
        if (coins[index] <= amount) {
            take = calMinCoins(coins, index, amount - coins[index]);
            if (take != Integer.MAX_VALUE) {
                take += 1;
            }
        }

        return take < notTake ? take : notTake;
    }
}
```

## DP - Top Down

```java
class Solution {

    int[][] dp;

    public int coinChange(int[] coins, int amount) {

        // min no of coins
        if (amount == 0) {
            return 0;
        }

        int n = coins.length;

        dp = new int[n][amount + 1];

        for (int[] row : dp) {
            Arrays.fill(row, -1);
        }

        calMinCoins(coins, n - 1, amount);

        return dp[n - 1][amount] != Integer.MAX_VALUE ? dp[n - 1][amount] : -1;
    }

    private int calMinCoins(int[] coins, int index, int amount) {

        if (amount == 0) {
            // no coins required
            return dp[index][amount] = 0;
        }

        if (index == 0) {
            if (amount % coins[index] == 0) {
                return dp[index][amount] = amount / coins[index];
            }
            return dp[index][amount] = Integer.MAX_VALUE; 
        }

        if (dp[index][amount] != -1) {
            return dp[index][amount];
        }

        int notTake = calMinCoins(coins, index - 1, amount);

        int take = Integer.MAX_VALUE;
        if (coins[index] <= amount) {
            take = calMinCoins(coins, index, amount - coins[index]);
            if (take != Integer.MAX_VALUE) {
                take += 1;
            }
        }

        return dp[index][amount] = take < notTake ? take : notTake;
    }
}
```

## DP - Bottom Up

```java
class Solution {

    int[][] dp;

    public int coinChange(int[] coins, int amount) {

        // min no of coins
        if (amount == 0) {
            return 0;
        }

        int n = coins.length;

        dp = new int[n][amount + 1];

        for (int[] row : dp) {
            Arrays.fill(row, -1);
        }

        for (int i = 0; i < n; i++) {
            dp[i][0] = 0;
        }

        for (int j = 1; j <= amount; j++) {
            if (j % coins[0] == 0) {
                dp[0][j] = j / coins[0];
            } else {
                dp[0][j] = Integer.MAX_VALUE;
            }
        }

        for (int i = 1; i < n; i++) {
            for (int j = 1; j <= amount; j++) {

                int notTake = dp[i - 1][j];

                int take = Integer.MAX_VALUE;

                if (coins[i] <= j) {
                    take = dp[i][j - coins[i]];

                    if (take != Integer.MAX_VALUE) {
                        take += 1;
                    }
                }

                dp[i][j] = Math.min(notTake, take);
            }
        }

        return dp[n - 1][amount] != Integer.MAX_VALUE ? dp[n - 1][amount] : -1;
    }
}
```
