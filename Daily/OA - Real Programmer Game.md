See question here [1point3arces](https://www.1point3acres.com/bbs/thread-690296-1-1.html)

DP

Time Complexity: O(NMK)

```java
// "static void main" must be defined in a public class.

import java.util.Scanner;
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int numTestCases = Integer.parseInt(sc.nextLine());
        while(numTestCases-- > 0){
            int[] input = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();
            int totalHealth = input[0], damageLimit = input[1], totalAttacks = input[2];
            
            // already dead
            if(totalHealth <= 0){
                Systme.out.println("1");
                continue;
            }
            
            // cannot beat
            if(damageLimit <= 0 || totalAttacks <= 0 || damageLimit * totalAttacks < totalHealth){
                Systme.out.println("0");
                continue;
            }
            
            double[][] dp = new double[totalHealth+1][totalAttacks+1];
            dp[0][0] = 1;
            
            for(int remainAttack = 1; remainAttack <= totalAttacks; remainAttack++){
                int lowestHealth = totalHealth - (totalAttacks - remainAttack) * damageLimit;
                lowestHealth = lowestHealth <= 0 ? 0 : lowestHealth;
                while(lowestHealth <= totalHealth){
                    for(int damage = 0; damage <= damageLimit; damage++){
                        if(lowestHealth <= damage){
                            dp[lowestHealth][remainAttack] += 1;
                        } else {
                            dp[lowestHealth][remainAttack] += dp[lowestHealth - damage][remainAttack - 1];   
                        }
                    }
                    dp[lowestHealth][remainAttack] /= (damageLimit+1);
                    lowestHealth++;
                }
            }
            
            for(int i = 0; i <= totalHealth; i++){
                for(int j = 0; j <= totalAttacks; j++){
                    System.out.print(dp[i][j] + "  ");
                }
                System.out.println();
            }
            System.out.println(dp[totalHealth][totalAttacks]);
        }
    }
}
```
