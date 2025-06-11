https://www.geeksforgeeks.org/problems/top-view-of-binary-tree/1

* We need to use BFS approach, level by level order traversal, Need to use the concept of Horizontal distance
* Here DFS (recursion) will not work, bcz at same index, lower element can get visited first

Note: We can use pair class also instead of NodeIndex </br>
<b>Queue<Pair<Node, Integer>> q = new LinkedList<>(); </b>

```java
class NodeIndex{
    Node root;
    int index;
    
    NodeIndex(Node root, int index){
        this.root = root;
        this.index = index;
    }
} 

class Solution
{
    //Function to return a list of nodes visible from the top view 
    //from left to right in Binary Tree.
    static ArrayList<Integer> topView(Node root)
    {
        // add your code
        ArrayList<Integer> result = new ArrayList<>();
        
        Queue<NodeIndex> queue = new LinkedList<>();
        
        queue.add(new NodeIndex(root, 0));
        
        Map<Integer, Integer> indexMap = new TreeMap<>();
        
        while(!queue.isEmpty()){
            
            NodeIndex nodeIndex = queue.poll();
            Node node = nodeIndex.root;
            //if(!indexMap.containsKey(nodeIndex.index)){
            indexMap.putIfAbsent(nodeIndex.index, node.data);
            //}
            
            if(node.left!=null){
                queue.add(new NodeIndex(node.left, nodeIndex.index-1));   
            }
            
            if(node.right!=null){
                queue.add(new NodeIndex(node.right, nodeIndex.index+1));   
            }
        }
        
        for(Integer val : indexMap.values()){
            result.add(val);
        }
        
        return result;
    }
}
```