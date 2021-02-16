See question here [1point3acres](https://www.1point3acres.com/bbs/thread-690298-1-1.html). 

### Union Find

Time Complexity: O(N)

```java
// "static void main" must be defined in a public class.
public class Main {
    
    private static int find(int start, int[] trans){
        if(trans[start] == start)
            return start;
        return find(trans[start], trans);
    }
    
    private static int numInCycle(int start, int[] trans){
        if(start == -1)
            return 0;
        if(trans[start] == start)
            return 1;
        return 1 + numInCycle(trans[start], trans);
    }
    
    public static int minTransferChar(String source, String target){
        System.out.println(" --- ");
        if(source == null){
            System.out.println("Invalid input!");
            return target == null ? 0 : -1;
        }
        int len = source.length();
        if(len != target.length()){
            System.out.println("Different length of two inputs!");
            return -1;
        }
        if(len <= 1 || source.equals(target)){
            System.out.println("Same string!");
            return 0;   
        }
        
        // initialize the trans array
        int[] visited = new int[26];
        int[] trans = new int[26];
        for(int i = 0; i < 26; i++)
            trans[i] = i;
        int[] cycleStart = new int[26];
        Arrays.fill(cycleStart, -1);
        
        for(int i = 0; i < len; i++){
            int start = source.charAt(i)-'a', end = target.charAt(i)-'a';
            if(start == end)
                continue;
            
            visited[start] = 1; visited[end] = 1;
            
            if(trans[start] != start && trans[start] != end){
                System.out.println("One character cannot be transfered to two different character!");
                return -1;   
            } else if (trans[start] == end){
                // visited relationship
                continue;
            }
            
            int root = find(end, trans);
            // cycle detected
            if(trans[start] == root){
                trans[start] = start; // instead of allocate 'end' to trans[start], allocate 'start' for breaking the cycle, which also make the 'start' as the root of the component.
                cycleStart[start] = end;
            } else {
                trans[start] = end;   
            }
            
            System.out.println("start = " + start + " , end = " + end + " , trans[start] = " + trans[start]);
            
        }
        
        int[] numInComponent = new int[26];
        List<Integer> rootList = new LinkedList<>();
        for(int i = 0; i < 26; i++){
            if(visited[i] == 1){
                int componentRoot = find(i, trans);
                numInComponent[componentRoot]++;
                if(componentRoot == i){
                    rootList.add(i);   
                }   
            }
        }
        
        int count = 0;
        for(int root : rootList){
            int num = numInCycle(cycleStart[root], trans);
            char rootChar = (char)('a' + root);
            
            // Difference between the following 3 situcations is the key.
            if(num == 0){
                System.out.println("Component Root " + rootChar + ": no cycle exists in this component");
                count += numInComponent[root] - 1;
            } else if(numInComponent[root] == num){
                System.out.println("Component Root " + rootChar + ": all node in this component belong to the same cycle");
                count += num+1;
            } else if(numInComponent[root] > num){
                System.out.println("Component Root " + rootChar + ": extra node not in the cycle, so we can utilize extra node to make room for breaking the cycle");
                count += numInComponent[root];
            } else {
                System.out.println("Error: numInComponent[root] < num is impossible");
            }
        }
        
        return count;
        
    }
    
    public static void main(String[] args) {
        System.out.println("Hello World!");
        
        System.out.println("Ans = " + minTransferChar("aa","aaa"));
        System.out.println("Ans = " + minTransferChar("aaa","aaa"));
        System.out.println("Ans = " + minTransferChar("bb","de"));
        
        System.out.println("Ans = " + minTransferChar("aaa","bbb"));
        System.out.println("Ans = " + minTransferChar("ababcc","cccccc"));
        System.out.println("Ans = " + minTransferChar("ab","ba"));
    }
}
```
