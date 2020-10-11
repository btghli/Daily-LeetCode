###### Sort

### OA - Minimum steps to sort an array by only placing the number to the left or right side

Given an unsorted array. You can only place one number to the left or right at one step.  
What is the minimum steps to sort the array?


##### Example 1:
```
3 4 1 2 5

2 to the left: 2 3 4 1 5
1 to the left: 1 2 3 4 5 
```
###### Output 2


##### Example 2:  
```
1 5 3 2 4

2 to the left:  2 1 5 3 4
1 to the left:  1 2 5 3 4  
5 to the right: 1 2 3 4 5
```
###### Output 3


Notice that:  
`nums[i]` can be larger than the `nums.length`

***


```java
// "static void main" must be defined in a public class.
public class Main {
    
    public static int minSteps(int[] nums){
        
        // record the index of original nums
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < nums.length; i++)
            map.put(nums[i], i+1);
        
        // sort the nums to get the correct order
        Arrays.sort(nums);
        
        // pick the middle number as the pivot
        // the larger number should be its right side, the smaller number should be its left side
        // lo: the index of the smallest visited (already placed to the correct position) number
        // hi: the index of the largest visited number
        int lo = map.get(nums[nums.length / 2]), hi = map.get(nums[nums.length / 2]);
        int steps = 0;
        
        for(int i = nums.length / 2 - 1; i >= 0; i--){
            if(map.get(nums[i]) > lo){
                lo = lo > 0 ? 0 : lo - 1;
                steps++;
            } else {
                lo = map.get(nums[i]);
            }
        }
        
        for(int i = nums.length / 2 + 1; i < nums.length; i++){
            if(map.get(nums[i]) < hi){
                hi = hi < nums.length ? nums.length + 1 : hi + 1;
                steps++;
            } else {
                hi = map.get(nums[i]);
            }
        }
        
        return steps;
        
    }
    
    public static void main(String[] args) {
        // int[] input = new int[]{3,2,1,4,5};
        int[] input = new int[]{1,4,3,2,5};
        System.out.println(minSteps(input));
    }
}
```
