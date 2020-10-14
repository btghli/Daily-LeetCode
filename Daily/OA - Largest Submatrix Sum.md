### OA - Largest Submatrix Sum
[via: Blog](https://hjweds.gitbooks.io/leetcode/content/largest-submatrix-sum.html)

Given a matrix that contains integers, find the submatrix with the largest sum.  

Return the sum of the submatrix.  

###### Assumptions  

The given matrix is not null and has size of M * N, where M > = 1 and N > = 1

###### Examples
```
{ {1, -2,-1, 4},

{1, -1, 1, 1},

{0, -1,-1, 1},

{0, 0, 1, 1} }

the largest submatrix sum is (-1) + 4 + 1 + 1 + (-1) + 1 + 1 + 1 = 7.
```

###### Solution: loop through i, j组成的matrix，压成array，然后求largest subarray sum

***

```java
public int largest(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length, result = matrix[0][0];
    
    // loop i from 0 ~ m to check possible submatrix start i to j
    for (int i = 0; i < m; i++) {
      
      // for every i, start a new cur array to store the sum of number from row i ~ j of column k
      int[] cur = new int[n];
      
      for (int j = i; j < m; j++) {
        for (int k = 0; k < n; k++) {
          cur[k] += matrix[j][k];
        }
        
        //Algorithm about largest subarray sum
        int min = 0, max = cur[0], preSum = 0;
        for (int num : cur) {
          preSum += num;
          max = Math.max(max, preSum - min);
          min = Math.min(min, preSum);
        }
        
        result = Math.max(result, max);
      }
    }
    return result;
  }
  ```
