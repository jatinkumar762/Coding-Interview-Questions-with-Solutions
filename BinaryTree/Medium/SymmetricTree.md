
https://leetcode.com/problems/symmetric-tree/description/
https://takeuforward.org/data-structure/check-for-symmetrical-binary-tree/

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        
        if(root==null) return true;

        return isSymmetric(root.left, root.right);
    }

    private boolean isSymmetric(TreeNode left, TreeNode right){

        if(left==null && right==null) return true;

        if(left==null || right==null) return false;

        if(left.val!=right.val) return false;

        return isSymmetric(left.left, right.right) && isSymmetric(left.right, right.left);
    }
}
```

### Using Stack

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {

        if (root == null)
            return true;

        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode left, right;

        if (root.left != null) {
            if (root.right == null)
                return false;
            stack.push(root.left);
            stack.push(root.right);
        } else if (root.right != null) {
            return false;
        }

        while (!stack.empty()) {

            if (stack.size() % 2 != 0)
                return false;

            right = stack.pop();
            left = stack.pop();

            if (right.val != left.val)
                return false;

            if (left.left != null) {
                if (right.right == null)
                    return false;
                stack.push(left.left);
                stack.push(right.right);
            } else if (right.right != null) {
                return false;
            }

            if (left.right != null) {
                if (right.left == null)
                    return false;
                stack.push(left.right);
                stack.push(right.left);
            } else if (right.left != null) {
                return false;
            }
        }

        return true;
    }
}
```