https://www.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1

```java
class Solution {
    
    static int[][] dp;
    
    static int knapsack(int W, int val[], int wt[]) {
        // code here
        
        int len = val.length;
        
        dp = new int[len][W+1];
        
        for(int[] row : dp){
            Arrays.fill(row, -1);
        }
        
        for(int i = 0; i <len; i++){
            dp[i][0] = 0;
        }
        
        maxProfit(len - 1, W, val, wt);
        
        return dp[len-1][W];
    }
    
    static int maxProfit(int index, int W, int val[], int wt[]){
        
        if(W <= 0){
            return 0;
        }
        
        if(index == 0){
            
            if(wt[index] <= W){
                return dp[index][W] = val[index];
            }   
            
            return dp[index][W] = 0;
        }
        
        if(dp[index][W]!=-1){
            return dp[index][W];
        }
        
        int take = Integer.MIN_VALUE;
        
        if(wt[index] <= W){
            take = val[index] + maxProfit(index - 1, W - wt[index], val, wt);
        }
        
        int notTake = maxProfit(index - 1, W, val, wt);
        
        
        return dp[index][W] = (take > notTake ? take : notTake);
    }
}
```

---

https://www.geeksforgeeks.org/problems/knapsack-with-duplicate-items4201/1

### Recursive

```java
class Solution{
    static int knapSack(int N, int W, int val[], int wt[])
    {
        // code here
        return calMaxProfit(wt, val, N-1, W);
    }
    
    static int calMaxProfit(int[] wt, int[] val, int index, int W){
        
        
        if(W == 0){
            return 0;
        }
        
        if(index < 0 || W < 0){
            return Integer.MIN_VALUE;
        }
        
        int notTake = calMaxProfit(wt, val, index-1, W);
        
        int take = Integer.MIN_VALUE;
        
        if(wt[index]<=W){
            take = val[index] + calMaxProfit(wt, val, index, W-wt[index]);
        }
        
        
        return take > notTake ? take : notTake;
    }
}
```

### Top-Down Approach (Memorization)

```java
class Solution{
    
    static int[][] dp;
    
    static int knapSack(int N, int W, int val[], int wt[])
    {
        // code here
        dp = new int[N][W+1];
        
        for(int[] row : dp){
            Arrays.fill(row, -1);
        }
        
        calMaxProfit(wt, val, N-1, W);
        
        return dp[N-1][W];
    }
    
    static int calMaxProfit(int[] wt, int[] val, int index, int W){
        
        
        if(index < 0 || W < 0){
            return Integer.MIN_VALUE;
        }
        
        if(W == 0){
            return dp[index][W] = 0;
        }
        
        if(dp[index][W]!=-1){
            return dp[index][W];
        }
        
        int notTake = calMaxProfit(wt, val, index-1, W);
        
        int take = Integer.MIN_VALUE;
        
        if(wt[index]<=W){
            take = val[index] + calMaxProfit(wt, val, index, W-wt[index]);
        }
        
        
        return dp[index][W] = (take > notTake ? take : notTake);
    }
}
```

### Bottom-Up (Tabulation)

```java
class Solution{
    
    static int[][] dp;
    
    static int knapSack(int N, int W, int val[], int wt[])
    {
        // code here
        dp = new int[N+1][W+1];
        
        for(int i = 0; i<=N ; i++){
            dp[i][0] = 0;
        }
        
        for(int i=1; i<=N ; i++){
            for(int j=1; j<=W; j++){
                int notTake = dp[i-1][j]; //notTake
                
                int take = Integer.MIN_VALUE;
                if(wt[i-1]<=j){
                    take = val[i-1] + dp[i][j-wt[i-1]];
                }
                
                dp[i][j] = take > notTake ? take: notTake;
            }
        }
        
        return dp[N][W];
    }
}
```

**To-Do:** 1-D Array Space Optimized Approach