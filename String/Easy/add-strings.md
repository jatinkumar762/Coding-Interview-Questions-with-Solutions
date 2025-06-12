https://leetcode.com/problems/add-strings/description/

```java
import java.math.BigInteger;

class Solution {
    public String addStrings(String num1, String num2) {

        //BigInteger number = new BigInteger("0");

        StringBuilder res = new StringBuilder("");

        int i = num1.length() - 1;
        int j = num2.length() - 1;

        int carry = 0;

        while (i >= 0 || j >= 0) {

            int x1 = i >= 0 ? num1.charAt(i) - 48 : 0;
            int x2 = j >= 0 ? num2.charAt(j) - 48 : 0;

            int sum = x1 + x2 + carry;

            carry = sum / 10;

            sum = sum % 10;

            res.append(sum);
            //number = number.multiply(BigInteger.valueOf(10)).add(BigInteger.valueOf(sum));

            i--;
            j--;
        }

        if (carry != 0) {
            res.append(carry);
        }

        res.reverse();

        return res.toString();
    }
}
```