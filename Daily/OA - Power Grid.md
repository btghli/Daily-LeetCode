### Power Grid

See question here [GitHub - ama-oa2-ng/PowerGrid](https://github.com/BrandonHo/Technical-Coding-Questions/blob/a6f11cdcba1468997b54503bc94dace31cb98293/src/ama-oa2-ng/PowerGrid.java)


Kruskal Algorithm for MST

```java
// "static void main" must be defined in a public class.
public class Main {
    
    static class UF{
        
        int[] parent; 
        
        public UF(int num){
            parent = new int[num];
            for(int i = 0; i < num; i++){
                parent[i] = i;
            }
        }
        
        public boolean union(int a, int b){
            int A = find(a), B = find(b);
            if(A != B){
                int min = Math.min(A, B);
                parent[A] = min;
                parent[B] = min;
                return true;
            }
            return false;
        }
        
        public int find(int a){
            if(parent[a] == a)
                return a;
            return find(parent[a]);
        }
    }
    
    public static void print(int[][] nums){
        for(int i = 0; i < nums.length; i++){
            for(int j = 0; j < nums[0].length; j++){
                System.out.print(nums[i][j] + " ");
            }
            System.out.println();
        }
    }
    
    // Kruskal Algorithm for MST
    public static int[][] powerGrid(int num, int[][] graph){
        Arrays.sort(graph, (i1,i2) -> i1[2] - i2[2]);
        // Collections.sort(graph, (i1,i2) -> i1[2] - i2[2]);
        print(graph);
        
        System.out.println("-----------");
        
        List<int[]> res = new LinkedList<>();
        UF uf = new UF(num);
        for(int[] edge : graph){
            if(uf.union(edge[0], edge[1])){
               res.add(edge);
            }
        }

        int[][] ans = new int[res.size()][3];
        int i = 0;
        for(int[] r : res){
            ans[i++] = Arrays.copyOf(r,3);
            // ans[i++] = r;
        }
        
        print(ans);
        return ans;
    }
    
    public static void main(String[] args) {
        int num = 5;
        int[][] graph = new int[5][3];
        graph[0][0] = 0; graph[0][1] = 1; graph[0][2] = 1;
        graph[1][0] = 1; graph[1][1] = 2; graph[1][2] = 4;
        graph[2][0] = 1; graph[2][1] = 3; graph[2][2] = 6;
        graph[3][0] = 3; graph[3][1] = 4; graph[3][2] = 5;
        graph[4][0] = 2; graph[4][1] = 4; graph[4][2] = 1;
        powerGrid(num, graph);
    }
}
```
