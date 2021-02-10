###### DP

### Split String Into Unique Primes

See question here: [GitHub - ama-oa2-ng/SplitStringIntoUniquePrimes](https://github.com/BrandonHo/Technical-Coding-Questions/blob/a6f11cdcba1468997b54503bc94dace31cb98293/src/ama-oa2-ng/SplitStringIntoUniquePrimes.java)

Time Complexity: O(N^3) where `N` is the length of the input string.  
Space Complexity: O(N^2)  

According to: 
```
Constraints:
1 <= length of inputStrings <= 10^5
```

`N` is at most `6`.

```java
// "static void main" must be defined in a public class.
public class Main {
    
    public static boolean isPrime(int num){
        if(num <= 1)
            return false;
        if(num == 2 || num == 3)
            return true;
        for(int i = 2; i*i <= num; i++){
            if(num % i == 0)
                return false;
        }
        return true;
    }
    
    public static int countWaysSplitingString(String s){
        int MOD = 1000000007;
        int len = s.length();
        int[][] dp = new int[len][len];
        boolean[][] prime = new boolean[len][len];
        for(int i = 0; i < len; i++){
            prime[i][i] = isPrime(Integer.parseInt(String.valueOf(s.charAt(i))));
            dp[i][i] = prime[i][i] ? 1 : 0;
        }
        for(int i = len-2; i >= 0; i--){
            for(int j = i + 1; j < len; j++){
                prime[i][j] = isPrime(Integer.parseInt(String.valueOf(s.substring(i,j+1))));
                dp[i][j] = prime[i][i] ? 1 : 0;
                for(int k = i; k < j; k++){
                    dp[i][j] += prime[i][k] ? dp[k+1][j] : 0;
                }
                dp[i][j] %= MOD;
            }
        }
        return dp[0][len-1];
    }
    
    public static void test(){
        int test1 = countWaysSplitingString("373");
        int test2 = countWaysSplitingString("11373");
        int test3 = countWaysSplitingString("433");
        
        System.out.println(test1);
        System.out.println(test2);
        System.out.println(test3);
    }
    
    public static void main(String[] args) {
        test();
    }
}
```

Improvement:
- Use `HashSet` to replace the matrix `prime`, so that reduce the calling of `isPrime`
- Use `HashSet` to replace the matrix `dp` by storing visited `substring` and corresponding `count` as key-value
