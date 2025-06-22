https://leetcode.com/problems/serialize-and-deserialize-bst/description

* use pre-order to serialize the binary tree

**Note**

* we can use level order traversal to serialize, but will be complex to create tree 

```java
public class Codec {

    // Encodes a tree to a single string.
    private int index = -1;

    public String serialize(TreeNode root) {
        
        StringBuilder strBuilder = new StringBuilder("");

        preOrder(root, strBuilder);

        return strBuilder.toString();
    }

    private void preOrder(TreeNode root, StringBuilder str) {

        if (root == null) {

            str.append("-1 ");

            return;
        }

        str.append(root.val + " ");

        preOrder(root.left, str);

        preOrder(root.right, str);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {

        if (data.length() == 0) {
            return null;
        }

        String[] arr = data.trim().split(" ");

        System.out.println(Arrays.toString(arr));

        int len = arr.length;

        return buildTree(arr, len);
    }

    private TreeNode buildTree(String[] arr, int len) {

        index++;

        if (index >= len || Integer.parseInt(arr[index]) == -1) {
            return null;
        }

        TreeNode node = new TreeNode(Integer.parseInt(arr[index]));

        node.left = buildTree(arr, len);

        node.right = buildTree(arr, len);

        return node;
    }
}
```

---

* pre-order using stack, but recursion is faster

```java
public class Codec {

    // Encodes a tree to a single string.
    private int index = -1;

    public String serialize(TreeNode root) {

        if (root == null) {
            return "";
        }

        StringBuilder strBuilder = new StringBuilder("");

        Stack<TreeNode> stack = new Stack<>();

        while (root != null || !stack.isEmpty()) {

            while (root != null) {

                strBuilder.append(root.val + " ");

                stack.push(root);

                root = root.left;
            }

            strBuilder.append("-1 ");

            if (!stack.isEmpty()) {

                root = stack.pop();

                root = root.right;
            }
        }

        return strBuilder.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {

        if (data.length() == 0) {
            return null;
        }

        String[] arr = data.trim().split(" ");

        System.out.println(Arrays.toString(arr));

        int len = arr.length;

        return buildTree(arr, len);
    }

    private TreeNode buildTree(String[] arr, int len) {

        index++;

        if (index >= len || Integer.parseInt(arr[index]) == -1) {
            return null;
        }

        TreeNode node = new TreeNode(Integer.parseInt(arr[index]));

        node.left = buildTree(arr, len);

        node.right = buildTree(arr, len);

        return node;
    }
}
```