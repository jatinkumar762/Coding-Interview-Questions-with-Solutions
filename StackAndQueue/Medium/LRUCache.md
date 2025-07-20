https://leetcode.com/problems/lru-cache/description/

### Approach-1 Using Inbuilt doubly linked list and Map - 1031 ms

```java
class LRUCache {

    LinkedList<Integer> dLL;
    Map<Integer, Integer> keyToValue;
    int capacity;
    int count;

    public LRUCache(int capacity) {
        dLL = new LinkedList<>();
        this.capacity = capacity;
        keyToValue = new HashMap<>();
        this.count = 0;
    }

    //O(capacity)
    public int get(int key) {

        if (!keyToValue.containsKey(key)) {
            return -1;
        }

        // if directly pass int, then it will consider index not object
        dLL.remove(Integer.valueOf(key));

        dLL.addFirst(key);

        return keyToValue.get(key);
    }

    //O(capacity)
    public void put(int key, int value) {

        if (keyToValue.containsKey(key)) {
            dLL.remove(Integer.valueOf(key));
        } else {
            if (count == capacity) {
                keyToValue.remove(dLL.removeLast());
            } else {
                count += 1;
            }
        }

        keyToValue.put(key, value);
        dLL.addFirst(key);
    }
}
```

### Approach-2 Custom Doubly Linked List - 47ms

```java
class Node {
    int key;
    int val;
    Node prev;
    Node next;

    Node(int key, int val) {
        this.key = key;
        this.val = val;
        prev = null;
        next = null;
    }
}

class LRUCache {

    Node head, tail;
    Map<Integer, Node> keyToValue;
    int capacity;
    int count;

    public LRUCache(int capacity) {
        head = tail = null;
        this.capacity = capacity;
        keyToValue = new HashMap<>();
        this.count = 0;
    }

    public int get(int key) {

        if (!keyToValue.containsKey(key)) {
            return -1;
        }

        remove(keyToValue.get(key));

        add(keyToValue.get(key));

        //print(head);

        return keyToValue.get(key).val;
    }

    public void put(int key, int value) {

        Node tmp;

        if (keyToValue.containsKey(key)) {
           
            tmp = keyToValue.get(key);

            remove(tmp);

            tmp.val = value;
            
        } else {
            if (count == capacity) {
                keyToValue.remove(tail.key);
                remove(tail);
            } else {
                count += 1;
            }
            tmp = new Node(key, value);
            keyToValue.put(key, tmp);
        }

        add(tmp);

        //print(head);
    }

    private void add(Node node) {
        if (head == null) {
            head = tail = node;
        } else {
            node.next = head;
            head.prev = node;
            head = node;
        }
    }

    // Remove a node from the list
    private void remove(Node node) {

        // tmp node
        if (node.next != null && node.prev != null) {

            node.prev.next = node.next;
            node.next.prev = node.prev;

        } else {

            if (node.next == null) {
                // means last node;
                tail = node.prev;

                if (tail != null) {
                    tail.next = null;
                }
            }

            if (node.prev == null) {
                // means head node
                head = node.next;

                if (head != null) {
                    head.prev = null;
                }
            }
        }

        node.prev = null;
        node.next = null;
    }

    private void print(Node h) {
        Node tmp = h;

        while (h != null) {
            System.out.print(h.key + " ");
            h = h.next;
        }

        System.out.println("Hello");
    }
}
```

### Approach - 3 More clean code than approach - 2

```java
class Node {
    int key;
    int val;
    Node prev;
    Node next;

    Node(int key, int val) {
        this.key = key;
        this.val = val;
        prev = null;
        next = null;
    }
}

class LRUCache {

    Node head, tail;
    Map<Integer, Node> keyToValue;
    int capacity;
    int count;

    public LRUCache(int capacity) {
        head = tail = new Node(-1, -1);
        head.next = tail;
        tail.prev = head;

        this.capacity = capacity;
        this.count = 0;

        keyToValue = new HashMap<>();
    }

    public int get(int key) {

        if (!keyToValue.containsKey(key)) {
            return -1;
        }

        Node node = keyToValue.get(key);

        remove(node);

        add(node);

        return node.val;
    }

    public void put(int key, int value) {

        if (keyToValue.containsKey(key)) {

            remove(keyToValue.get(key));

        } else {

            if (count == capacity) {

                keyToValue.remove(tail.prev.key);

                remove(tail.prev);
            } else {

                count += 1;
            }
        }

        Node node = new Node(key, value);

        keyToValue.put(key, node);

        add(node);
    }

    private void add(Node node) {

        Node nextNode = head.next;

        head.next = node;

        node.prev = head;
        node.next = nextNode;

        nextNode.prev = node;
    }

    // Remove a node from the list
    private void remove(Node node) {
        Node prevNode = node.prev;
        Node nextNode = node.next;
        prevNode.next = nextNode;
        nextNode.prev = prevNode;

        node.prev = null;
        node.next = null;
    }
}
```

### Approach - 4 Using In-built library

```java

import java.util.*;
import java.lang.*;

class LRUCache<K,V> {
    
    private int capacity;
    private LinkedHashMap<K, V> map;
    
    public LRUCache(int capacity){
        
        this.capacity = capacity;
        
        map = new LinkedHashMap<>(capacity, 0.75f, true){
            
            protected boolean removeEldestEntry(Map.Entry<K, V> entry){
                
                return size() > capacity;   
                
            }
        };
    }
    
    public synchronized void put(K key, V value){
        map.put(key, value);
    }
    
    public synchronized V get(K key){
        return map.getOrDefault(key, null);
    }
}


public class Main
{
	public static void main(String[] args) {
	    
	    LRUCache<Integer, Integer> lruCache = new LRUCache<>(2);
	    
	    lruCache.put(1, 1);
	    lruCache.put(2, 2);
	    
	    System.out.println(lruCache.get(1));
	    
	    lruCache.put(3, 3);
	    
	    System.out.println(lruCache.get(2));
	    
	    System.out.println(lruCache.get(3));
	    
	    System.out.println("*********************");
	    
	    LRUCache<Integer, String> lruCacheStr = new LRUCache<>(2);
	    
	    lruCacheStr.put(1, "1");
	    lruCacheStr.put(2, "2");
	    
	    System.out.println(lruCacheStr.get(1));
	    
	    lruCacheStr.put(3, "3");
	    
	    System.out.println(lruCacheStr.get(2));
	    
	    System.out.println(lruCacheStr.get(3));
	}
}
```