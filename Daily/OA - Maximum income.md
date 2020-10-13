```java
package demo;

import java.util.*;

class Solution{
    
    public static void main(String[] args) {
        @SuppressWarnings("resource")
		Scanner sc = new Scanner(System.in);

        Map<Integer, Integer> driverA = new HashMap<>(), driverB = new HashMap<>();
        Queue<Integer> incomeA = new PriorityQueue<>((i1, i2) -> 
                                 (driverA.get(i2) != driverA.get(i1) ? 
                                  driverA.get(i2) - driverA.get(i1) : driverB.get(i1) - driverB.get(i2))),
                       incomeB = new PriorityQueue<>((i1, i2) -> 
                                 (driverB.get(i2) != driverB.get(i1) ? 
                                  driverB.get(i2) - driverB.get(i1) : driverA.get(i1) - driverA.get(i2)));

        int driver = 0;
        System.out.println("start");
        while (sc.hasNext()) {
            String[] next = sc.nextLine().split(" ");
            
            
            for(String s : next) {
            	System.out.print(s + " ");
            }
            
            if(next[0] == "#") {
            	System.out.println("end of input");
            }
            int[] income = Arrays.stream(next).mapToInt(Integer::parseInt).toArray();

            driverA.put(driver, income[0]);
            incomeA.add(driver);

            driverB.put(driver, income[1]);
            incomeB.add(driver);

            System.out.println(driver + " " + income[0] + " " + income[1]);
            
            driver++;
        }

        System.out.println("end of input");
        
        int sum = 0;
        Set<Integer> visited = new HashSet<>();
        while(!incomeA.isEmpty() || !incomeB.isEmpty()){
            int currDriver = -1;

            while(!incomeA.isEmpty() && visited.contains(incomeA.peek())){
                incomeA.poll();
            }

            while(!incomeB.isEmpty() && visited.contains(incomeB.peek())){
                incomeB.poll();
            }

            if(incomeA.isEmpty() && incomeB.isEmpty())
                break;

            if(incomeA.isEmpty()){
                    currDriver = incomeB.poll();
                    sum += driverB.get(currDriver);
                
            } else if(incomeB.isEmpty()){
                    currDriver = incomeA.poll();
                    sum += driverA.get(currDriver);
                
            } else {
                if(driverA.get(incomeA.peek()) > driverB.get(incomeB.peek())){
                    currDriver = incomeA.poll();
                    sum += driverA.get(currDriver);
                } else {
                    currDriver = incomeB.poll();
                    sum += driverB.get(currDriver);
                }

            }
            visited.add(currDriver);
            System.out.println(currDriver);
        }

        System.out.print(sum);
    }

}
```
