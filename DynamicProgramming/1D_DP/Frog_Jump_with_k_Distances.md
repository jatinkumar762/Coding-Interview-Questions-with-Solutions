https://www.geeksforgeeks.org/problems/minimal-cost/1

#### Top-Down

```java
class Solution {
    
    int[] dp;
    
    public int calculateMinEnergy(int[] arr, int index, int k)
    {
        if(index==0){
            dp[index] = 0;
            return dp[index];
        }
        
        if(dp[index]!=-1){
            return dp[index];
        }
        
        int res = Integer.MAX_VALUE;
        
        for(int i=1;i<=k;i++) {
            int cost = Integer.MAX_VALUE;
            if(index-i>=0){
                cost = Math.abs(arr[index] - arr[index-i]) + calculateMinEnergy(arr, index-i, k);
            }
            res = res<cost?res:cost;
        }
        
        dp[index] = res;
        
        return dp[index];
    }
       
       
    public int minimizeCost(int arr[], int k) {
        // code here
        int N = arr.length;
        
        dp = new int[N];
        
        Arrays.fill(dp, -1);
        
        return calculateMinEnergy(arr, N-1, k);
    }
}
```

#### Bottom-Up

```java
class Solution {
    int[] dp;
       
    public int minimizeCost(int k, int arr[]) {
        // code here
        int N = arr.length;
        
        dp = new int[N];
        
        dp[0] = 0;
        
        for(int i = 1; i < N; i++){
            
            int res = Integer.MAX_VALUE, cost = Integer.MAX_VALUE;

            for(int j=1; (i- j) >= 0 && j<=k; j++) {

                cost = dp[i-j] + Math.abs(arr[i] - arr[i-j]);
                
                res = res < cost ? res : cost;
            }
            
            dp[i] = res;
        }
        
        return dp[N-1];
    }
}
```