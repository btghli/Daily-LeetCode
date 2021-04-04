[OA Uber](https://www.1point3acres.com/bbs/thread-696384-1-1.html)

You are given a square matrix of characters a which size is n x n.   
Your task is to create a list of strings from the diagonals of a, where each string has a length of n, and then sort this created list.  
We'll consider all diagonals that are parallel to the main diagonal, where each diagonal is considered to start at its upper point and end at its lower point.   
Since these diagonals will have different lengths, we'll traverse each one cyclically (ie: go back to the start of the diagonal after reaching the end) until we reach n characters.  
Sort the resulting strings in lexicographical order, and return an array of 2n - 1 integers, representing the diagonals' 1-based indices in their sorted order.   
In the case of lexicographically equal strings, their indices should be kept in the original order.  
Here's an example of how to count the diagonals for a 5 x 5 matrix (the number on the diagram corresponds to the diagonal index):  
```
5 6 7 8 9 
4 5 6 7 8 
3 4 5 6 7 
2 3 4 5 6 
1 2 3 4 5
```

###### Example
```
• For a = [["b", "b"],     ["c", "a"]]
the output should be diagonalsArranging(a) = [2, 3, 1].
    
This matrix has n = 2 and contains 3 diagonals:
  ○ The diagonal with index 1 is ["c"] and its corresponding cyclic string is "cc";
  ○ The diagonal with index 2 is ["b", "a"] and its corresponding cyclic string is "ba";
  ○ The diagonal with index 3 is ["b"] and its corresponding cyclic string is "bb".The lexicographical ordering of the matrix diagonals looks like ["ba", "bb", "cc"], so the answer is [2, 3, 1].
  
```
```
• Fora = [["a", "c", "a", "b", "b"],      ["c", "b", "a", "c", "b"],      ["a", "a", "e", "c", "b"],      ["b", "b", "d", "a", "g"],      ["a", "b", "e", "b", "a"]]the output should be diagonalsArranging(a) = [1, 5, 3, 7, 2, 8, 9, 6, 4].
       
This matrix has n = 5 and contains 9 diagonals:
  ○ The diagonal with index 1 is ["a"] and its corresponding cyclic string is "aaaaa",
  ○ The diagonal with index 2 is ["b", "b"] and its corresponding cyclic string is "bbbbbb",
  ○ The diagonal with index 3 is ["a", "b", "e"] and its corresponding cyclic string is "abeab",
  ○ The diagonal with index 4 is ["c", "a", "d", "b"] and its corresponding cyclic string is "cadbc",
  ○ The diagonal with index 5 is ["a", "b", "e", "a", "a"] and its corresponding cyclic string is "abeaa",
  ○ The diagonal with index 6 is ["c", "a", "c", "g"] and its corresponding cyclic string is "cacgc",
  ○ The diagonal with index 7 is ["a", "c", "b"] and its corresponding cyclic string is "acbac",
  ○ The diagonal with index 8 is ["b", "b"] and its corresponding cyclic string is "bbbbb",
  ○ The diagonal with index 9 is ["b"] and its corresponding cyclic string is "bbbbb".The lexicographical ordering of the matrix diagonals looks like ["aaaaa", "abeaa", "abeab", "acbac", "bbbbb", "bbbbb", "bbbbb", "cacgc", "cadbc"], so the answer is [1, 5, 3, 7, 2, 8, 9, 6, 4].Note that the cyclic string "bbbbb" occurs 3 times, and the indices corresponding to this cyclic string appear in ascending order in the output: [2, 8, 9].
```

###### Input/Output
• [execution time limit] 4 seconds (py3)
• [input] array.array.char aA square matrix of characters. It is guaranteed that the characters in the matrix are all lowercase English letters.Guaranteed constraints:2 ≤ a.length ≤ 100,a[i].length = a.length.
• [output] array.integerAn array of size 2n - 1, containing the indices of the diagonals after sorting.


```java
// "static void main" must be defined in a public class.
public class Main {
    
    public static int[] getDiag(String[][] input){
        int len = input.length;
        int x = len-1, y = 0, idx = 1;
        
        // Use List<Integer> as the value in the case of duplicate diagonals
        Map<String, List<Integer>> diagToIdx = new HashMap<>();
        Queue<String> lexicographical = new PriorityQueue<>();
        StringBuilder sb = new StringBuilder();
        while(idx <= 2*len-1){
            sb.setLength(0);
            int nextX = x, nextY = y;
            while(sb.length() < len){
                sb.append(input[nextX++][nextY++]);
                
                // if next cell out of boundary, move back to the start cell
                if(nextX >= len || nextY >= len){
                    nextX = x;
                    nextY = y;
                }
            }
            
            System.out.println(idx + " : " + sb.toString());
            diagToIdx.computeIfAbsent(sb.toString(), v -> new LinkedList<>()).add(idx++);
            lexicographical.offer(sb.toString());
            
            // move to next diagonal
            if(x-1 >= 0){ // move upward
                x--;
            } else { // move to the right 
                y++;
            }
        }
        int[] res = new int[idx-1];
        idx = 0;
        String previous = "";
        while(!lexicographical.isEmpty()){
            String curr = lexicographical.poll();
            
            // avoid duplicate diagonals in the lexicographical queue
            if(curr.equals(previous))
                continue;
            for(int i : diagToIdx.get(curr)){
                res[idx++] = i;
            }
            previous = curr;
        }
        return res;
    }
    
    public static void main(String[] args) {
        String[][] input = {{"a", "c", "a", "b", "b"},
                            {"c", "b", "a", "c", "b"},
                            {"a", "a", "e", "c", "b"},
                            {"b", "b", "d", "a", "g"},
                            {"a", "b", "e", "b", "a"}};
        int[] res = getDiag(input);
        for(int r : res)
            System.out.print(r + " ");
    }
}
```
