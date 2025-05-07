https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/


```java
class Solution {
    public int maxProfit(int[] prices) {
        
        int len = prices.length;

        int max = prices[len-1];
        int res = 0;

        for(int i = len-2; i>=0; i--){

            res = Math.max(res, max - prices[i]);

            max = Math.max(max, prices[i]);
        }

        return res;
    }
}
```

**Optimization**

```java
class Solution {
    public int maxProfit(int[] prices) {

        int len = prices.length;

        int max = prices[len - 1];
        int res = 0;

        for (int i = len - 2; i >= 0; i--) {

            if (prices[i] > max) {
                max = prices[i];
            } else {
                res = Math.max(res, max - prices[i]);
            }
        }

        return res;
    }
}
```

---

```java
public class Solution {
    public int maxProfit(int prices[]) {
        int minprice = Integer.MAX_VALUE;
        int maxprofit = 0;
        for (int i = 0; i < prices.length; i++) {
            if (prices[i] < minprice) minprice = prices[i];
            else if (prices[i] - minprice > maxprofit) maxprofit = prices[i] -
            minprice;
        }
        return maxprofit;
    }
}
```