https://leetcode.com/problems/copy-list-with-random-pointer/description/


### Using Map

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/

class Solution {
    public Node copyRandomList(Node head) {
        
        Map<Node, Node> copyMap = new HashMap<>();

        Node tmp = head;

        while(tmp!=null){

            copyMap.put(tmp, new Node(tmp.val));

            tmp = tmp.next;
        }

        tmp = head;

        while(tmp!=null){

            Node copy = copyMap.get(tmp);

            copy.next = copyMap.get(tmp.next);

            copy.random = copyMap.get(tmp.random);

            tmp = tmp.next;
        }

        return copyMap.get(head);
    }
}
```