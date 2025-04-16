https://www.geeksforgeeks.org/problems/job-sequencing-problem-1587115620/1

#### Approach

* for max profit, we need to pick max profit job first
* hence, sort the jobs in descending order of profit
* Now say if a job has a deadline of 4 we can perform it anytime between day 1-4, but it is preferable to perform the job on its last day.
* this leaves enough empty slots on the previous days to perform other jobs.
* if last day not possible, will check day before last day to perform job


```java
class Solution
{
    //Function to find the maximum profit and the number of jobs done.
    int[] JobScheduling(Job arr[], int n)
    {
        // Sort jobs by descending profit
        //Arrays.sort(arr, new JobSort());
        Arrays.sort(jobs, (a, b) -> b.profit - a.profit);
        
        int[] jobPosition = new int[n+1];
        
        int count=0, profit=0;
        int index;
        for(int i=0;i<n;i++) {
            
            index = arr[i].deadline;
            while(index>=1){
                if(jobPosition[index]==0){
                  break;  
                } 
                index--;
            }
            if(index>0) {
                count++;
                jobPosition[index] = arr[i].id;
                profit+=arr[i].profit;
            }
        }
        
        return new int[]{count, profit};
    }
}
```