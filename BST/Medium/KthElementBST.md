https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/

* perform in-order traversal

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
    public int kthSmallest(TreeNode root, int k) {
        
        Stack<TreeNode> stack = new Stack<>();

        while(root!=null || stack.size()>0){

            while(root!=null){
                stack.add(root);
                root = root.left;
            }

            root = stack.pop();
            k-=1;

            if(k==0)
                return root.val;

            if(root!=null){
                root = root.right;
            }
        }
        return -1;
    }
}
```