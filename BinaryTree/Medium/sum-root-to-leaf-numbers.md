https://leetcode.com/problems/sum-root-to-leaf-numbers/description/

### Recursion

```java
class Solution {

    public int sumNumbers(TreeNode root) {

        return calSum(root, 0);
    }

    public int calSum(TreeNode root, int sum) {

        if (root == null) {
            return 0;
        }

        if (root.left == null && root.right == null) {
            return (sum * 10 + root.val);
        }

        int left = calSum(root.left, (sum * 10) + root.val);

        int right = calSum(root.right, (sum * 10) + root.val);

        return left + right;
    }
}
```

---

```java
class Solution {

    int globalSum = 0;

    public int sumNumbers(TreeNode root) {

        calSum(root, 0);

        return globalSum;
    }

    public void calSum(TreeNode root, int sum) {

        if (root == null) {
            return;
        }

        if (root.left == null && root.right == null) {
            globalSum += (sum * 10 + root.val);
        }

        calSum(root.left, (sum * 10) + root.val);

        calSum(root.right, (sum * 10) + root.val);
    }
}
```

### Pre-Order Traversal

