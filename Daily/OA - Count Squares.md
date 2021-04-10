Given a matrix which contains `0` and `1`, return the number of squares containing only 1.

###### Example 1:
```
3 * 3 matrix contains only 1
Nine 1 * 1 square
Four 2 * 2 square
One 3 * 3 square

Total 14
```
 
For a N * N square input matrix:  
Time Complexity: O(N ^ 4)

```java
public class Main {
    public static int squaresInMatrix(int[][] matrix){
        int m = matrix.length, n = matrix[0].length, count = 0;
        
        // n^2 for every cell as the left-up corner of the square
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(matrix[i][j] == 1){


                    // n^2 for possible max square
                    int max = -1, x = 0;
                    while(max == -1 || (i+x < m && x < max)){

                        int y = 0;
                        while(max == -1 || y < max){

                            // when j+y reach the boundry, compute max
                            if(j+y == n){
                                max = max == -1 ? y : Math.min(max, y);
                                break;
                            }

                            // no need to continue when y > max
                            if(max != -1 && y >= max){
                                break;
                            }

                            // when j+y reach a "0", compute max
                            if(matrix[i+x][j+y] == 0){
                                max = max == -1 ? y : Math.min(max, y);
                                break;
                            }

                            y++;
                        }

                        // when we got max from y, we have 3 situations about x
                        // x < max: not reach the potential max square
                        // x == max: curr square is the max square, break
                        // x > max: max square is limited by y, break
                        if(x >= max){
                            max = x;
                            break;
                        }

                        x++;

                        // i+y reach the boundry, compute max and break
                        if(i+x == m){
                            max = x;
                            break;
                        }
                    }
                    count += max *(max+1)*(2*max+1)/6;
                    System.out.println("[" + i + "," + j + "] : " + count);

                    // eliminate the visited max square
                    for(x = 0; x < max; x++){
                        for(int y = 0; y < max; y++){
                            matrix[i+x][j+y] = 0;
                        }
                    }
                }
            }
        }
        
        return count;
    }
    
    public static void main(String[] args) {
        System.out.println("Hello World!");
        int[][] matrix = {{1,1,1,1,1},
                          {1,1,1,1,1},
                          {1,1,1,1,1},
                          {1,1,1,1,1},
                          {1,1,1,1,1}};
        
        // int[][] matrix = {{1,1,1,1,1},
        //                   {1,1,1,1,1},
        //                   {1,1,1,1,1}};
        
        // int[][] matrix = {{1,1,1},
        //                   {1,1,1},
        //                   {1,1,1}};
        System.out.println(squaresInMatrix(matrix));
    }
}
```

For a M * N input matrix:   

Brute Force  
Time Complexity: O(N ^ 5)
```java
public class Main {
    public static int squaresInMatrix(int[][] matrix){
        int m = matrix.length, n = matrix[0].length, limit = Math.min(m,n), count = 0;
        
        // for n possible length of square
        for(int len = 1; len <= limit; len++){
            
            // n^2 for every cell as the left-up corner of the square
            for(int i = 0; i < m; i++){
                for(int j = 0; j < n; j++){
                    if(matrix[i][j] == 1){
                        boolean isSquare = true;
                        for(int x = 0; x < len && isSquare; x++){
                            for(int y = 0; y < len && isSquare; y++){
                                if(i+x >= m ||  j+y >= n){
                                    isSquare = false;
                                    break;
                                }
                                isSquare &= matrix[i+x][j+y] == 1;
                            }
                        }
                        
                        if(isSquare)
                            count++;
                        
                        // System.out.println("len: " + len + " [" + i + "," + j + "] : " + count);
                    }
                }
            }   
        }
        
        return count;
    }
    
    public static void main(String[] args) {
        System.out.println("Hello World!");
        // int[][] matrix = {{1,1,1,1,1},
        //                   {1,1,1,1,1},
        //                   {1,1,1,1,1},
        //                   {1,1,1,1,1},
        //                   {1,1,1,1,1}};
        
//         int[][] matrix = {{1,1,1,1,1},
//                           {1,1,1,1,1},
//                           {1,1,1,1,1}};
        
//         int[][] matrix = {{1,1,1},
//                           {1,1,1},
//                           {1,1,1}};
        
        // int[][] matrix = {{1,1,1},
        //                   {1,1,1}};
        
        // int[][] matrix = {{1}};
        System.out.println(squaresInMatrix(matrix));
    }
}
```
