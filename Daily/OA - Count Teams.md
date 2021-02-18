### OA - Count Teams

Given a list of workers' skill levels with desired upper and lower bounds, determine how many teams can be created from the list.

###### Example
```
Input:
  num = 6
  skills = [12, 4, 6, 13, 5, 10]
  minAssociates = 3
  minLevel = 4
  maxLevel = 10
  
Output:
  5
```

###### Constraints
- 1 <= num <= 20
- 1 <= minAssociates <= num
- 1 <= minLevel <= maxLevel <= 1000
- 1 <= skills[i] <= 1000
- 0 <= i < num

***

```java
public class Main {
    public static int countTeams(int num, List<Integer> skills, int minAssociates, int minLevel, int maxLevel) {
        int count = 0;
        for (int i : skills) {
            if (i >= minLevel && i <= maxLevel) {
                count ++;
            }
        }
        if (count < minAssociates) {
            return 0;
        }
        int res = 0;
        for (int i = minAssociates; i <= count; i ++) {
            res += countCombination(count, i);
        }
        return res;
    }
    
    private static int countCombination(int n, int m) {
        if (m == 0 || n == m) {
            return 1;
        }
        return countCombination(n - 1, m) + countCombination(n - 1, m - 1);
    }
    
    public static void main(String[] args) {
        int num = 5;
        
        List<Integer> skills = new ArrayList<>();
        skills.add(12);
        skills.add(4);
        skills.add(6);
        skills.add(13);
        skills.add(5);
        skills.add(10);
        
        int minAssociates = 3;
        int minLevel = 4;
        int maxLevel = 10;
        
        int res = countTeams(num, skills, minAssociates, minLevel, maxLevel);
        System.out.println(res);
    }
}
```
