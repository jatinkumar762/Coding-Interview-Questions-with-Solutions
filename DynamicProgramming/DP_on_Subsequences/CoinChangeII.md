https://leetcode.com/problems/coin-change-ii/description/

* count total no of ways &rarr; base case will return 1 or 0


### Recursion 

```java
class Solution {
    public int change(int amount, int[] coins) {

        int n = coins.length;

        return calCombinations(coins, n - 1, amount);
    }

    private int calCombinations(int[] coins, int index, int amount) {

        // Input: amount = 5, coins = [1,2,5]
        // Output: 4
        
        if (amount == 0) {
            // no coins required - 1 way - empty subset
            return 1;
        }

        //1 <= coins[i] <= 5000 not 0
        //so we can stop
        if (index == 0) {
            if (amount % coins[index] == 0) {
                return 1; // 1 way
            }
            return 0;
        }

        int notTake = calCombinations(coins, index - 1, amount);

        int take = 0;
        if (coins[index] <= amount) {
            take = calCombinations(coins, index, amount - coins[index]);
        }

        return take + notTake;
    }
}
```

**another way to write**

```java
private int countCombinations(int[] coins, int index, int amount) {
    // Base case: exact amount is achieved
    if (amount == 0) {
        return 1;
    }
    // Base case: negative amount or no coins left
    if (amount < 0 || index < 0) {
        return 0;
    }
    
    // Count combinations by excluding the coin
    int excludeCoin = countCombinations(coins, index - 1, amount);
    
    // Count combinations by including the coin
    int includeCoin = countCombinations(coins, index, amount - coins[index]);
    
    // Total combinations
    return excludeCoin + includeCoin;
}
```

### Top-Down (Memorization)

```java
class Solution {

    int[][] dp;

    public int change(int amount, int[] coins) {

        int n = coins.length;

        dp = new int[n][amount + 1];

        for (int[] row : dp) {
            Arrays.fill(row, -1);
        }

        calCombinations(coins, n - 1, amount);

        return dp[n - 1][amount];
    }

    private int calCombinations(int[] coins, int index, int amount) {

        // Input: amount = 5, coins = [1,2,5]
        // Output: 4

        if (amount == 0) {
            // no coins required - 1 way - empty subset
            return dp[index][amount] = 1;
        }

        if (index == 0) {
            if (amount % coins[index] == 0) {
                return dp[index][amount] = 1; // 1 way
            }
            return dp[index][amount] = 0;
        }

        if (dp[index][amount] != -1) {
            return dp[index][amount];
        }

        int notTake = calCombinations(coins, index - 1, amount);

        int take = 0;
        if (coins[index] <= amount) {
            take = calCombinations(coins, index, amount - coins[index]);
        }

        return dp[index][amount] = take + notTake;
    }
}
```

### Bottom-Up (Tabulation)

```java
class Solution {

    public int change(int amount, int[] coins) {

        int n = coins.length;

        int[][] dp = new int[n][amount + 1];

        for (int i = 0; i < n; i++) {
            dp[i][0] = 1;
        }

        for (int j = 0; j <= amount; j++) {
            if (j % coins[0] == 0) {
                dp[0][j] = 1;
            }
        }

        for (int i = 1; i < n; i++) {
            for (int j = 1; j <= amount; j++) {
                dp[i][j] = dp[i - 1][j];

                if (coins[i] <= j) {
                    dp[i][j] += dp[i][j - coins[i]];
                }
            }
        }

        return dp[n - 1][amount];
    }
}
```