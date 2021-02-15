```java
// "static void main" must be defined in a public class.

import java.util.Scanner;
public class Main {
    public static class ComponentNode{
        public int value;
        public ArrayList<ComponentNode> components;
        public ComponentNode(){
            components = new ArrayList<ComponentNode>();
        }
        public ComponentNode(int numOfLinesChanged){
            this.value = numOfLinesChanged;
            this.components = new ArrayList<ComponentNode>();
        }
    }
    
    static double max = 0;
    static ComponentNode res = null;
    
    public static ComponentNode getNode(ComponentNode root){
        if(root == null || root.components == null || root.components.size() == 0)
            return root;
        long[] ans = getAverage(root);
        return res;
    }
    
    public static long[] getAverage(ComponentNode root){
        if(root.components == null || root.components.size() == 0)
            return new long[]{1,root.value};
        
        long cur_num = 1;
        long cur_sum = root.value;
        // int[0] = num of nodes in subtree, int[1] = avg
        for(ComponentNode node : root.components){
            long[] sub = getAverage(node);
            cur_num += sub[0];
            cur_sum += sub[1];
        }
        
        if(cur_sum / cur_num > max){
            max = (double)cur_sum / (double)cur_num;
            res = root;
        }
        return new long[]{cur_num, cur_sum};
    }
    
    public static void main(String[] args) {
        ComponentNode root1 = new ComponentNode(1);
        ComponentNode root2 = new ComponentNode(2);
        ComponentNode root3 = new ComponentNode(3);
        root1.components.add(root2);
        root2.components.add(root3);
        System.out.println(getNode(root1).value);
        
        ComponentNode root12 = new ComponentNode(12);
        ComponentNode root10 = new ComponentNode(10);
        ComponentNode root9 = new ComponentNode(9);
        ComponentNode root0 = new ComponentNode(0);
        root12.components.add(root0);
        root12.components.add(root10);
        root10.components.add(root9);
        System.out.println(getNode(root12).value);
        
    }
}
```
