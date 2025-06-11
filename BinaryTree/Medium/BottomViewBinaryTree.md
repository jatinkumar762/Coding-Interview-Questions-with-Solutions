1. https://takeuforward.org/data-structure/bottom-view-of-a-binary-tree/
2. https://www.geeksforgeeks.org/problems/bottom-view-of-binary-tree/0

Note: We can use pair class also instead of NodeIndex </br>
<b>Queue<Pair<Node, Integer>> q = new LinkedList<>(); </b>

We use a map data structure to store the nodes corresponding to each vertical level of the tree as the map automatically sorts the elements based on their ascending value. Against each vertical level, the node lowest in the tree at that vertical level is added by traversing the tree level order wise (BFS).

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
    //Function to return a list containing the bottom view of the given tree.
    public ArrayList <Integer> bottomView(Node root)
    {
        // Code here
        ArrayList<Integer> result = new ArrayList<>();
        
        Queue<NodeIndex> queue = new LinkedList<>();
        queue.add(new NodeIndex(root, 0));
        
        Map<Integer, Integer> indexMap = new TreeMap<>();
        
        while(!queue.isEmpty()){
            
            NodeIndex nodeIndex = queue.poll();
            Node node = nodeIndex.root;
            
            indexMap.put(nodeIndex.index, node.data);
            
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