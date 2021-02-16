See question here [1point3acres](https://www.1point3acres.com/bbs/thread-690296-1-1.html)

```java
// "static void main" must be defined in a public class.

import java.util.Scanner;
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int numTestCases = Integer.parseInt(sc.nextLine());
        while(numTestCases-- > 0){
            int[] input = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();
            
            // Too much difference will waste time and may lead to TLE. Truncate the larger number so that the two input numbers are on the same dimension.
            int diff = Math.abs(input[0]-input[1]);
            int countSec = (int)Math.sqrt(diff * 2) - 1;
            if(input[0] > input[1]){
                input[0] -= (1+countSec) * countSec / 2;
            } else {
                input[1] -= (1+countSec) * countSec / 2;
            }
            countSec++;
            
            Queue<Integer> pq = new PriorityQueue<>((a, b) -> input[b]==input[a]?a-b:input[b]-input[a]);
            pq.offer(0); pq.offer(1);
            
            while(input[pq.peek()] >= countSec){
                int idx = pq.poll();
                input[idx] -= countSec++;
                pq.offer(idx);
            }
            System.out.println(countSec + " " + input[0] + " " + input[1]);
        }
    }
}
```
