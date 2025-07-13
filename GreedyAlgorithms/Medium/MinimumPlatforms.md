https://www.geeksforgeeks.org/problems/minimum-platforms-1587115620/1

**Similar Problem** - https://leetcode.com/problems/meeting-rooms-ii/description/

#### Approach-1 Brute Force - $O(N^2)$

* pick each train, and check its arrival time is not between with other train arrival and departure time -> coinciding or not

```java
class Solution
{
    //Function to find the minimum number of platforms required at the
    //railway station such that no train waits.
    static int findPlatform(int arr[], int dep[], int n)
    {
        // add your code here
        int plat_needed = 1, result = 1;

        // run a nested  loop to find overlap   
        for (int i = 0; i < n; i++) {
            // minimum platform
            plat_needed = 1;

            for (int j = 0; j < n; j++) {
                if (i != j)
                    // check for overlap
                    //If the arrival time of a train is between the arrival and departure 
                    //times of another train, it is considered as an overlap and a counter is incremented.
                    if (dep[i] >= arr[j] && dep[j] >= arr[i])
                        plat_needed++;
            }

            // update result
            result = Math.max(result, plat_needed);
        }

        return result;
    }
    
}
```

#### Approach-2 $O(NlogN)$

* will see trains according to timeframe
* if any train come -> platform required
* if any train depart -> platform will free

Input: n = 6, arr[] = {0900, 0940, 0950, 1100, 1500, 1800}, 
            dep[] = {0910, 1200, 1120, 1130, 1900, 2000}

(0900, A) - (0910, D) - (0940, A) - (0950, A) - (1100, A) - (1120, D) - .......

```java
class Solution
{
    //Function to find the minimum number of platforms required at the
    //railway station such that no train waits.
    static int findPlatform(int arr[], int dep[], int n)
    {
        // add your code here
        Arrays.sort(arr);
        Arrays.sort(dep);
        
        int aIndex=0, dIndex=0, platform=0;
        
        int maxPlatform=0;
        
        while(aIndex<n && dIndex<n){
            
            if(arr[aIndex] <= dep[dIndex]){
                platform++;
                aIndex++;
            } else {
                //dept is less
                //train will leave before arrival of another train
                platform--;
                dIndex++;
            }
            
            maxPlatform = Math.max(platform, maxPlatform); 
        }
        
        //if any remain -> they will only dep arr elements
        //bcz arrival < dep
        //max will find in above loop only 
        return maxPlatform;
    }
    
}
```
