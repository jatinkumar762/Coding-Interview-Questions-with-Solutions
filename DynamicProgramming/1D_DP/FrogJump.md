https://www.geeksforgeeks.org/problems/geek-jump/1

#### Recursive Solution - TLE

```java
class Solution{
    
    public int calculateEnergy(int[] arr, int currIndex, int N){
        
        if(currIndex>=N-1){
            return 0;
        }
        
        int res1 = Integer.MAX_VALUE, res2 = Integer.MAX_VALUE;
        
        if(currIndex+1<=N-1){
            res1 = Math.abs(arr[currIndex] - arr[currIndex+1]) + calculateEnergy(arr, currIndex + 1, N);
        }
        
        if(currIndex+2<=N-1){
            res2 =  Math.abs(arr[currIndex] - arr[currIndex+2]) + calculateEnergy(arr, currIndex + 2, N);
        }
        
        return Math.min(res1, res2);
    }
    
    public int minimumEnergy(int arr[],int N){
        
        //code here
        
        return calculateEnergy(arr, 0, N);
    }
}
```

#### Top-Down Approach (Memorization) - StackOverflow error 

* left skewed tree - one jump
* right child is - 2 jumps

                5
            4       3
        3       2
    2       1
1


```java
class Solution{
    
    int[] dp;
    
    public int calculateMinEnergy(int[] arr, int index, int N)
    {
        if(index==0){
            dp[index] = 0;
            return dp[index];
        }
        
        if(dp[index]!=-1){
            return dp[index];
        }
        
        int e1 = Integer.MAX_VALUE, e2 = Integer.MAX_VALUE;
        
        e1 = Math.abs(arr[index] - arr[index-1]) + calculateMinEnergy(arr, index-1, N);
        
        if(index-2>=0){
            e2 = Math.abs(arr[index] - arr[index-2]) + calculateMinEnergy(arr, index-2, N);
        }
        
        dp[index] = e1 < e2 ? e1 : e2;
        
        return dp[index];
    }
    
    public int minimumEnergy(int arr[],int N){
        //code here
        dp = new int[N];
        
        Arrays.fill(dp, -1);
        
        return calculateMinEnergy(arr, N-1, N);
    }
}
```

**Time Complexity:** &rarr; $O(N)$

**Space Complexity:** &rarr; $O(N)$


#### Bottom-Up Approach (Tabulation)

* we can infer, bottom-up from top-down
* bottom goes till index-0, dp[0] = 0

```java
class Solution{
    
    int[] dp;
    
    public int minimumEnergy(int arr[],int N){
        //code here
        dp = new int[N];
        
        dp[0] = 0;
        
        int i;
        for(i=1;i<N;i++){
            
            if(i==1){
                dp[i] = dp[i-1] + Math.abs(arr[i] - arr[i-1]);
                continue;
            }
            
            int e1 = dp[i-1] + Math.abs(arr[i] - arr[i-1]);
            
            int e2 = dp[i-2] + Math.abs(arr[i] - arr[i-2]);
            
            dp[i] = e1 < e2 ? e1: e2;
        }
        
        return dp[N-1];
    }
}
```

#### Space Optimization

```java
class Solution{
    
    
    public int minimumEnergy(int arr[],int N){
        //code here
        
        int prev1 = 0, prev2 = 0, res = 0;
        
        
        for(int i=1;i<N;i++){
            
            if(i==1){
                res = prev1 + Math.abs(arr[i] - arr[i-1]);
                
                prev2 = prev1;
                prev1 = res;
                
                continue;
            }
            
            int e1 = prev1 + Math.abs(arr[i] - arr[i-1]);
            
            int e2 = prev2 + Math.abs(arr[i] - arr[i-2]);
            
            res = e1 < e2 ? e1 : e2;
            
            prev2 = prev1;
            prev1 = res;
        }
        
        return res;
    }
}
```


