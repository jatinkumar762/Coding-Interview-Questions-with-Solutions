https://leetcode.com/problems/binary-tree-level-order-traversal/description/

#### Approach-1

* this approach is based on count of nodes on each level
  - curCount = 1; nextCount = 0;


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
    public List<List<Integer>> levelOrder(TreeNode root) {
        
        List<List<Integer>> result = new ArrayList<>();

        if(root==null)
            return result; 

        int curCount = 1;
        int nextCount = 0;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);

        while(queue.size()>0) {

            List<Integer> levelResult = new ArrayList<>();
            for(int i=0;i<curCount;i++){

                TreeNode node = queue.remove();

                levelResult.add(node.val);

                if(node.left!=null){
                    nextCount+=1;
                    queue.add(node.left);
                }

                if(node.right!=null){
                    nextCount+=1;
                    queue.add(node.right);
                }
            }

            result.add(levelResult);

            curCount = nextCount;
            nextCount = 0;
        }

        return result;
    }
}
```