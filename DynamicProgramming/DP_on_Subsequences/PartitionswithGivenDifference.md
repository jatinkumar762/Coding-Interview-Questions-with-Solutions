https://www.geeksforgeeks.org/problems/partitions-with-given-difference/1


```
1. S1 + S2 = totalSum
2. in ques S1>=S2 and S1-S2 = d
3. from 1 -> S2 + d + S2 = totalSum
4. S2 = (totalSum-d)/2
```     

* this problem is extension of `DP - Perfect Sum Problem` 

```java
class Solution {
    
    static int modulo = 1000000007;
    static int[][] dp;
    
	private static int perfectSum(int arr[],int n, int sum) 
	{ 
	    dp = new int[n+1][sum+1];
	    
	    //base case initialization
	    for(int i = 0; i < n; i++){
	       dp[i][0] = 1; //empty subset
	    }
	    
	    for(int i = 1; i<=n; i++){
	        for(int j = 0; j<=sum; j++){
	            int not_take = dp[i-1][j]; //not take
	            
	            int take = 0;
	            if(arr[i-1]<=j){
	                take = dp[i-1][j-arr[i-1]]; //take
	            }
	            
	            dp[i][j] = (not_take + take)%modulo;
	        }
	    }
	    
	    return dp[n][sum];
	}
	
    public static int countPartitions(int n, int d, int[] arr) {
        // code here
        
        /*
            1. S1 + S2 = totalSum of arr elements
            2. in ques S1>=S2 and S1-S2 = d
            3. from 1 -> S2 + d + S2 = totalSum
            4. S2 = (totalSum-d)/2
        */
        
        
        int totalSum = 0;
        for(int ele : arr){
            totalSum+=ele;
        }
        
        if(totalSum-d<0 || (totalSum-d)%2!=0){
            return 0;
        }
        
        return perfectSum(arr, n, (totalSum-d)/2);
    }
}
```