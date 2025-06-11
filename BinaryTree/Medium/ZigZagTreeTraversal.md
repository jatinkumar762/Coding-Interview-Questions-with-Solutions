
https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/description/

### Optimized Code

```java

class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {

        if (root == null) {
            return result;
        }

				List<List<Integer>> result = new LinkedList<>();

        Queue<TreeNode> queue = new LinkedList<>();

        int count = 1;
        int newCount = 0;
        queue.add(root);

				boolean odd = true;

        while (!queue.isEmpty()) {

            List<Integer> levelResult = new LinkedList<>();

						//instead of newCount
						//we can initialize count = queue.size()

            for (int i = 0; i < count; i++) {

                TreeNode tmp = queue.poll();

                if (odd) {
                    levelResult.add(tmp.val);
                } else {
                    levelResult.addFirst(tmp.val);
                }

                if (tmp.left != null) {
                    queue.add(tmp.left);
                    newCount++;
                }

                if (tmp.right != null) {
                    queue.add(tmp.right);
                    newCount++;
                }

            }

            count = newCount;
            newCount = 0;
            odd = !odd;

            result.add(levelResult);
        }

        return result;
    }
}
```