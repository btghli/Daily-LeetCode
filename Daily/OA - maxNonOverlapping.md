Find the maximum number of non-intersecting segments of length 2

You are given a array of integers .Find the maximum number of non-intersecting segments of length 2(two adjacent elements ), such that segments have an equal sum.
Example 1:
Input: A= [10,1,3,1,2,2,1,0,4] 
Output: 3
Explanation: there are three non intersecting segments , each whose sum is equal to 4: (1,3), (2,2), (0,4).Another three non-intersecting segments are : (3,1), (2,2),(0,4)

Example 2:
Input: A = [5, 3, 1, 3, 2, 3]
Output: 1
Each sum of two adjacent elements is different from the others

Example 3:
Input: A = [9, 9, 9, 9, 9]
Output: 2

Example 4:
Input: A = [1, 5, 2, 4, 3, 3]
Output: 3
Explanaton: (1, 5), (2, 4), (3, 3), with sum of 6

###### Backtrack

```java
// "static void main" must be defined in a public class.
public class Main {
    static int max = 0;
    public static void backtrack(int[] nums, int start, Map<Integer, Integer> visited){
        if(start >= nums.length){
           for (Map.Entry<Integer,Integer> entry : visited.entrySet()){
               max = Math.max(max, entry.getValue());
           }
           return;
        }
        for(int i = start; i+1 < nums.length; i++){
            int sum = nums[i] + nums[i+1];
            visited.put(sum, visited.getOrDefault(sum, 0) + 1);
            backtrack(nums, i + 2, visited);
            visited.put(sum, visited.getOrDefault(sum, 0) - 1);
        }
    }
    
    public static int maxNonOverlapping(int[] nums) {
        backtrack(nums, 0, new HashMap<>());
        return max;
    }
    public static void main(String[] args) {
        System.out.println("Hello World!");
        // int[] nums = new int[]{10, 1, 3, 1, 2, 2, 1, 0, 4};
        // int[] nums = new int[]{5, 3, 1, 3, 2, 3};
        // int[] nums = new int[]{9, 9, 9, 9, 9};
        int[] nums = new int[]{1, 5, 2, 4, 3, 3};
        System.out.println(maxNonOverlapping(nums));
    }
}
```
