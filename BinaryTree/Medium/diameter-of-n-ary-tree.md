https://leetcode.com/problems/diameter-of-n-ary-tree/

### DFS

```java
class Solution {

    private int maxDiameter;

    public int diameter(Node root) {

        maxDiameter = 0;

        height(root);

        return maxDiameter;
    }

    private int height(Node root) {

        if (root == null) {
            return 0;
        }

        if (root.children == null || root.children.size() == 0) {
            return 0;
        }

        int first = 0;
        int second = 0;

        for (Node child : root.children) {

            int height = 1 + height(child);

            if (first < height) {
                second = first;
                first = height;
            } else if (second < height) {
                second = height;
            }
        }

        maxDiameter = Math.max(first + second, maxDiameter);

        return first;
    }
}
```