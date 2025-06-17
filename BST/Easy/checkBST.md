https://leetcode.com/problems/validate-binary-search-tree/description/

```java
class Solution {
    public boolean isValidBST(TreeNode root) {

        //return checkValid(root, Integer.MIN_VALUE, Integer.MAX_VALUE);
        return checkValid(root, null, null);
    }

    private boolean checkValid(TreeNode root, Integer min, Integer max) {

        if (root == null) {
            return true;
        }

        if ((min != null && root.val <= min) || (max != null && root.val >= max)) {
            return false;
        }

        return checkValid(root.left, min, root.val) && checkValid(root.right, root.val, max);
    }
}
```

```
[2,2,2] -> false

[2147483647] -> true

[-2147483648,null,2147483647] -> true
```

*  for optimization - without recursion - In order traversal using stack - and use property that during traversal current node val should be greater than prev node val

[Editorial](https://www.geeksforgeeks.org/a-program-to-check-if-a-binary-tree-is-bst-or-not/)