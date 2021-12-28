# 483. Smallest Good Base

### Problem:

For an integer n, we call k&gt;=2 a good base of n, if all digits of n base k are 1.

Now given a string representing n, you should return the smallest good base of n in string format.

Example 1:

```
Input: "13"
Output: "3"
Explanation: 13 base 3 is 111.
```

Example 2:

```
Input: "4681"
Output: "8"
Explanation: 4681 base 8 is 11111.
```

Example 3:

```
Input: "1000000000000000000"
Output: "999999999999999999"
Explanation: 1000000000000000000 base 999999999999999999 is 11.
```

Note:  
The range of n is \[3, 10^18\].  
The string representing n is always valid and will not have leading zeros.

### Solutions:

```java
lass Solution {
public:
    #define ll long long
    string smallestGoodBase(string n) {
        ll num = stoll(n);

        // let x be the good base of num
        // => 1 + x + x^2 + x^3 + ... + x^(m-1) = num
        // => x^m - 1 = num

        // the max value of m will be 61
        // explanation : 
        //      for m to be maximum, num will be 10^18 and x be 2
        //      2^m - 1 = 10^18 => 2^m = 10^18+1 ~ 10^18
        //      taking log both sides
        //      log(2^m) = log(10^18) => m*log2 = 18 => m = (18 / 0.3) => m = 61 (approx).

        for(int m = 61; m > 2; m--){
            ll lo = 2, hi = num-1;


            while(lo <= hi){
                ll mid =  (lo + hi) / 2;
                bool flag = 0;
                ll val = 1, j = 1, res = 1;
                while(j < m){
                    if(val  > (num - res) / mid){
                        flag = 1;
                        break;
                    }
                    j++;
                    val *= mid;
                    res += val;
                }

                if(flag || res > num){
                    hi = mid-1;
                }
                else if(res < num)
                    lo = mid + 1;
                else
                    return to_string(mid);
            }
        }
        return to_string(num-1);
    }
};


It is obviously the base b must meets n = b^k + b^(k-1) + ... + b^1 + 1
The straightforward way is to try from 2 to n, once it meets the formula above, we are done.
 But it will get TLE. To minimize the number of candidates, we do it from both directions: bottom-up and top-down.

For bottom-up: we set a LIMIT for k, as long as b^k + b^(k-1) + ... + b^1 + 1 > n, any larger number will not
 be able to form n with at least k+1 parts, in other word with b^k included. So, we are done.

For top-down: According to bottom-up result, just try from LIMIT-1 to 2.

If both ways not able to find a valid base, then the answer is n-1 because (n-1)^1 + 1 = n.

The value of LIMIT is used to balance the number of repeats between top-down and bottom-up loops

#define LIMIT 8
class Solution {
public:
    string smallestGoodBase(string n) {
        long num = stol(n);
        for (int base = 2; sum(base, LIMIT) <= num; base++) {
            if (valid(num, base)) {
                return to_string(base);
            }
        }
        
        for (int j = LIMIT-1; j > 1; j--) {
            long base = pow(num, 1.0/j);
            if (base < 2) continue;
            if (sum(base, j) == num) {
                return to_string(base);
            }
        }
        return to_string(num-1);
    }
    
    bool valid(long n, int base) {
        while(n) {
            if (n % base != 1) return false;
            n /= base;
        }
        return true;
    }
    
    long sum(int base, int k) {
        long res = 1;
        long num = 1;
        for (int i = 0; i < k; i++) {
            num *= base;
            res += num;
        }
        return res;
    }
};
```



