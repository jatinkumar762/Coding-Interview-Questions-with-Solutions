https://leetcode.com/problems/minimum-domino-rotations-for-equal-row/description/


### Leetcode Solution

Algorithm

Pick up the first element. It has two sides: A[0] and B[0].

Check if one could make all elements in A row or B row to be equal to A[0]. If yes, return the minimum number of rotations needed.

Check if one could make all elements in A row or B row to be equal to B[0]. If yes, return the minimum number of rotations needed.

Otherwise return -1.


```java
class Solution {
  /*
  Return min number of rotations 
  if one could make all elements in A or B equal to x.
  Else return -1.
  */
  public int check(int x, int[] A, int[] B, int n) {
    // how many rotations should be done
    // to have all elements in A equal to x
    // and to have all elements in B equal to x
    int rotations_a = 0, rotations_b = 0;
    for (int i = 0; i < n; i++) {
      // rotations couldn't be done
      if (A[i] != x && B[i] != x) return -1;
      // A[i] != x and B[i] == x
      else if (A[i] != x) rotations_a++;
      // A[i] == x and B[i] != x    
      else if (B[i] != x) rotations_b++;
    }
    // min number of rotations to have all
    // elements equal to x in A or B
    return Math.min(rotations_a, rotations_b);
  }

  public int minDominoRotations(int[] A, int[] B) {
    int n = A.length;
    int rotations = check(A[0], B, A, n);
    // If one could make all elements in A or B equal to A[0]
    if (rotations != -1 || A[0] == B[0]) return rotations;
    // If one could make all elements in A or B equal to B[0]
    else return check(B[0], B, A, n);
  }
}
```

### my approach

```java
class Solution {
    public int minDominoRotations(int[] tops, int[] bottoms) {

        int[] freq = new int[7];

        int len = tops.length;

        for (int i = 0; i < len; i++) {
            freq[tops[i]]++;
            freq[bottoms[i]]++;
        }

        int maxIndex = 1;
        for (int i = 2; i < 7; i++) {
            if (freq[i] > freq[maxIndex]) {
                maxIndex = i;
            }
        }

        for (int i = 0; i < len; i++) {
            if (tops[i] != maxIndex && bottoms[i] != maxIndex) {
                return -1;
            }
        }

        int topAns = 0;
        for (int i = 0; i < len; i++) {
            if (tops[i] != maxIndex) {
                topAns++;
            }
        }

        int bottomAns = 0;
        for (int i = 0; i < len; i++) {

            if (bottoms[i] != maxIndex) {
                bottomAns++;
            }
        }

        return topAns < bottomAns ? topAns : bottomAns;
    }
}
```