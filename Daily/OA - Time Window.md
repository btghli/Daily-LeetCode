input是一个timestamp的list，里面存的都是时间，不一定是sort了的，还有一个是time window大小
举个例子，timestamp是[101，102，130，139，200]， time window是 5，那time window这个区间可能是[101, 105],[102, 106]...[196,200]
让你找到 能包含最多个timestamp的那个time window，返回这个time window容纳的起始的那个timestamp
比如上面的例子最多的time window是[101,105]中有两个time stamp 101和102

```java
// "static void main" must be defined in a public class.
public class Main {
    public static int[] getTimeWindow(int[] timestamp, int timewindow){
        int[] res = new int[2];
        if(timestamp.length == 0){
            return res;
        } else if (timestamp.length == 0){
            res[0] = timestamp[0];
            res[1] = timestamp[0] + timewindow - 1;
            return res;
        }
        
        Arrays.sort(timestamp);
        int max = 0;
        
        // O(N) sliding window
        // int start = 0, end = 1;
        // while(end < timestamp.length){
        //     while(end < timestamp.length && timestamp[end] - timestamp[start] <= timewindow){
        //         end++;
        //     }
        //     if(end - start > max){
        //         max = end-start;
        //         res[0] = timestamp[start];
        //         res[1] = timestamp[start] + timewindow - 1;
        //         max = end-start;
        //     }
        //     start++;
        // }
        // if(end - start > max){
        //     res[0] = timestamp[start];
        //     res[1] = timestamp[start] + timewindow - 1;
        // }
        
        // O(NlogN) binary search
        for(int i = 0; i < timestamp.length; i++){
            int index = Arrays.binarySearch(timestamp, i+1, timestamp.length, timestamp[i]+timewindow);
            int len = 0;
            if(index < 0){
                index++;
                index = 0 - index;
                len = index - i;
            } else {
                len = index - i + 1;
            }
            
            // System.out.println("target: "+ (timestamp[i]+timewindow) + ", insertion index: " + index);
            
            if(len > max){
                res[0] = timestamp[i];
                res[1] = timestamp[i] + timewindow - 1;
                max = len;
            }
            
        }
        
        return res;
    }
    
    public static void main(String[] args) {
        int[] timestamp = {101, 102, 130, 139, 200};
        int timewindow = 5;
        int[] res = getTimeWindow(timestamp, timewindow);
        for(int r : res)
            System.out.println(r);
    }
}
```
