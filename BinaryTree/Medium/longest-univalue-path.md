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

### leetcode solution

```java
// Returns the length of the longest path (number of nodes) under the root
// that have the value same as the root. The path could either be
// on the left or right child of the root. The length includes the root as well.
class Solution {
    int ans;

    int solve(TreeNode root, int parent) {
        if (root == null) {
            return 0;
        }

        //The longest univalue path will cover nodes on both sides of the root.
        int left = solve(root.left, root.val);
        int right = solve(root.right, root.val);

        //The longest univalue path will cover nodes on both sides of the root.
        ans = Math.max(ans, left + right);

        // The number of nodes will be zero if the root value isn't equal to the root.
        // Otherwise return the max of left and right nodes plus one for the root itself.
        return root.val == parent ? Math.max(left, right) + 1 : 0;
    }

    public int longestUnivaluePath(TreeNode root) {
        // Use -1 for the parent value for the tree root node.
        solve(root, -1);

        return ans;
    }
}
```


Here, N is the number of nodes in the binary tree.

Time complexity: O(N)

We are iterating over each node only once and hence the time complexity is equal to O(N).

Space complexity: O(N)

The only space we need is during the recursion, the maximum number of active stack calls would be equal to the height of the tree. In the case of a skewed tree, the height of the tree will be equal to N, hence the space complexity is equal to O(N).