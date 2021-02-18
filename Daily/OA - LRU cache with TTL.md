See question at [1point3acres](https://www.1point3acres.com/bbs/forum.php?mod=viewthread&tid=203157&extra=&page=1)

```java
// "static void main" must be defined in a public class.

import java.util.Hashtable;
import java.lang.System;
public class Main {
    
    static class DLinkedNode{
        int key, value;
        long expireTime;
        DLinkedNode pre, post;
        
        public DLinkedNode(){
            
        }
        
        public DLinkedNode(int key, int value, long duration){
            this.key = key;
            this.value = value;
            this.expireTime = duration + System.currentTimeMillis();
        }
    }
    
    static class TTLCache{
        private Hashtable<Integer, DLinkedNode> cache;
        DLinkedNode head, tail;   
        
        public TTLCache(){
            cache = new Hashtable<Integer, DLinkedNode>();
            head = new DLinkedNode();
            head.key = -1;
            head.pre = null;
            
            tail = new DLinkedNode();
            tail.key = -2;
            tail.post = null;
            
            head.post = tail;
            tail.pre = head;
        }
        
        public void removeNode(DLinkedNode node){
            if(node == null || node == head)
                return;
            // System.out.println("remove " + node.key);
            node.post.pre = node.pre;
            node.pre.post = node.post;
            
            node.post = null;
            DLinkedNode previous = node.pre;
            node.pre = null;
            
            removeNode(previous);
        }
        
        public void addNode(DLinkedNode node){
            tail.pre.post = node;
            node.pre = tail.pre;
            tail.pre = node;
            node.post = tail;
        }
        
        public void put(int key, int value, long duration){
            DLinkedNode node = new DLinkedNode(key, value, duration);
            this.addNode(node);
            cache.put(key, node);
        }
        
        public int get(int key){
            DLinkedNode data = cache.get(key);
            long cur_time = System.currentTimeMillis();
            
            if(data == null || data.expireTime <= cur_time){
                this.removeNode(data);
                cache.remove(key);
                return -1;
            } else {
                return data.value;
            }
        }
    }
    
    
    public static void main(String[] args) {
        System.out.println("Hello World!");
        TTLCache cache = new TTLCache();
        cache.put(10, 10, 1);
        System.out.println("ans: " + cache.get(10));
        // Thread.sleep(4);
        cache.put(11, 11, 1);
        System.out.println("ans: " + cache.get(11));
        
        cache.put(12, 12, 1);
        System.out.println("ans: " + cache.get(12));
        cache.put(13, 13, 1);
        System.out.println("ans: " + cache.get(13));
        System.out.println("ans: " + cache.get(10));
        System.out.println("ans: " + cache.get(10));
        System.out.println("ans: " + cache.get(11));
        System.out.println("ans: " + cache.get(11));
        System.out.println("ans: " + cache.get(11));
    }
}
```
