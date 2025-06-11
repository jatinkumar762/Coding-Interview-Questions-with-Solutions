https://leetcode.com/problems/longest-univalue-path/description/

```java
class Solution {

    int maxResult = 0;

    public int longestUnivaluePath(TreeNode root) {

        findMaxPath(root);

        return maxResult;
    }

    public int findMaxPath(TreeNode root) {

        if (root == null) {
            return 0;
        }

        if (root.left == null && root.right == null) {
            return 0;
        }

        int left = findMaxPath(root.left);

        int right = findMaxPath(root.right);

        if (root.left != null && root.val == root.left.val) {
            left += 1;
        } else {
            left = 0;
        }

        if (root.right != null && root.val == root.right.val) {
            right += 1;
        } else {
            right = 0;
        }

        maxResult = Math.max(maxResult, left + right);

        return Math.max(left, right);
    }
}
```