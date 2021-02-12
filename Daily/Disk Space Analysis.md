### Disk Space Analysis

See question here [GitHub - ama-oa2-ng/DiskSpaceAnalysis](https://github.com/BrandonHo/Technical-Coding-Questions/blob/a6f11cdcba1468997b54503bc94dace31cb98293/src/ama-oa2-ng/DiskSpaceAnalysis.java)

```java
public class Main {

    public static void main(String[] args) {

        int[] hardDiskSpace = new int[] { 4, 2, 1, 7, 7, 2, 3 };
        int result = PerformDiskSpaceAnalysis(hardDiskSpace.length, hardDiskSpace, 2);
        System.out.println(result);
    }

    /*
     * Space Complexity: 
     * 
     * Time Complexity: 
     */
    public static int PerformDiskSpaceAnalysis(int numComputer, int[] hardDiskSpace, int segmentLength) {
        int max = -1;
        Deque<Integer> deque = new LinkedList<>();
        
        for(int i = 0; i < hardDiskSpace.length; i++){
            if(!deque.isEmpty() && i - deque.peekFirst() >= segmentLength)
                deque.poll();
            
            while(!deque.isEmpty() && hardDiskSpace[i] < hardDiskSpace[deque.peekLast()])
                deque.removeLast();
            
            deque.offerLast(i);
            if(i+1 >= segmentLength)
                max = Math.max(max, hardDiskSpace[deque.peekFirst()]);
            
            for(int a : deque)
                System.out.print(a + " ");
            System.out.println();
        }
        return max;
    }
}
```
