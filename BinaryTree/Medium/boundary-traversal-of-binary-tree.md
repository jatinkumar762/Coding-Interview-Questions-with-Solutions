https://www.geeksforgeeks.org/problems/boundary-traversal-of-binary-tree/1

https://leetcode.com/problems/boundary-of-binary-tree/description/

### Approach - 1

* left boundary traversal
* leaf node traversal - InOrder Traversal (LNR)
* right boundary traversal (add result in reverse)

```java
class Solution {
    ArrayList<Integer> boundaryTraversal(Node root) {
        // code here
        
        
        ArrayList<Integer> result = new ArrayList<>();
        
        if(root==null){
            return result;
        }
        
        result.add(root.data);
        
        if(root.left == null && root.right==null){
            return result;
        }
        
        //add left boundary
        Node tmp = root.left;
        while(tmp!=null && (tmp.right!=null || tmp.left!=null)){
            
            result.add(tmp.data);
            
            if(tmp.left!=null){
                tmp = tmp.left;
            } else {
                tmp =tmp.right;
            }
        }
        
        
        //add leaf nodes
        tmp = root;
        Stack<Node> stack = new Stack<>();
        while(tmp!=null || !stack.isEmpty()){
            
            while(tmp!=null){
                stack.push(tmp);
                tmp = tmp.left;
            }
            
            tmp = stack.pop();
            
            if(tmp.left == null && tmp.right == null){
                result.add(tmp.data);
            }
            
            tmp = tmp.right;
        }
        
        
        //add right boundary
        tmp = root.right;
        LinkedList<Integer> rightBoundary = new LinkedList<>();
        while(tmp!=null && (tmp.right!=null || tmp.left!=null)){
            
            rightBoundary.addFirst(tmp.data);
            
            if(tmp.right!=null){
                tmp = tmp.right;
            } else {
                tmp =tmp.left;
            }
        }
        
        rightBoundary.forEach(rb -> result.add(rb));
        
        return result;
    }
}
```