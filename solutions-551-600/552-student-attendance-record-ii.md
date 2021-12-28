# 552. Student Attendance Record II

### Problem:

Given a positive integer n, return the number of all possible attendance records with length n, which will be regarded as rewardable. The answer may be very large, return it after mod 10^9 + 7.

A student attendance record is a string that only contains the following three characters:

'A' : Absent.  
'L' : Late.  
'P' : Present.  
A record is regarded as rewardable if it doesn't contain more than one 'A' \(absent\) or more than two continuous 'L' \(late\).

Example 1:

```
Input: n = 2
Output: 8 
Explanation:
There are 8 records with length 2 will be regarded as rewardable:
"PP" , "AP", "PA", "LP", "PL", "AL", "LA", "LL"
Only "AA" won't be regarded as rewardable owing to more than one absent times.
```

Note: The value of n won't exceed 100,000.

### Solutions:

```java
This problem is not so hard... but you have to be aware of lots of details, like memory limit, number's out of range and etc.

First, I tried the DFS method. Of course, it is TLE.

class Solution {
public:
int checkRecord(int n) {
    long long res = 0;
    dfs(n, 0, 0, 0, res);
    return res % (int(pow(10,9))+7);
}
void dfs(int n, int s, int A, int L, long long& res){
    if(s == n){
        res++;
        return;
    }
    dfs(n, s+1, A, 0, res);//add "P"
    if(A < 1){
        dfs(n, s+1, A + 1, 0, res);
    }
    if(L <= 1){
        dfs(n, s+1,A, L + 1, res);
    }
}
};
So I switched to Dynamic Programming, the status functions are:
dp[n][j][k] : The current total length of our string is n, in current string s, the number of 'A' is j, of course j = 0 or 1, and the number of continuous L at the end of our string is k, k = 0, 1, 2.
Each time we will add one char at the end of our current string.

 (1) dp[n][0][0] = dp[n-1][0][0] + dp[n-1][0][1] + dp[n-1][0][2] 
 3 ways we can get length n string with 0 'A' and 0 continuous L at the end of string s: add 1 "P" at the end of  dp[n-1][0][0], dp[n-1][0][1], dp[n-1][0][2]

 (2) dp[n][0][1] = dp[n-1][0][0]
 We could only reach this status from dp[n-1][0][0] by add one 'L'

 (3) dp[n][0][2] = dp[n-1][0][1] 
 We could only reach this status from dp[n-1][0][1] by add one 'L'
  
 (4) dp[n][1][0] = dp[n-1][0][0] (add one 'A' after the 'P') + 
                     dp[n-1][0][1](add one 'A' after the 'L') + 
                     dp[n-1][0][2](add one 'A' after the 'LL')
                     dp[n-1][1][0](add one 'P' after 'P') + 
                     dp[n-1][1][1](add one 'P' after 'L') + 
                     dp[n-1][1][2](add one 'P' after 'LL')


 (5) dp[n][1][0] = dp[n-1][0][0] (add one 'A' after 'P') + 
                      dp[n-1][0][1](add one 'A' after 'L') + 
                      dp[n-1][0][2](add one 'A' after 'LL')
                      dp[n-1][1][0](add one 'P' after 'P') + 
                      dp[n-1][1][1](add one 'P' after 'L') + 
                      dp[n-1][1][2](add one 'P' after 'LL')
              
 (6) dp[n][1][1] = dp[n-1][1][0]
  (add one 'L' after 'P' or 'A', and at n-1, we've already have one 'A' in our string)

 (7) dp[n][1][2] = dp[n-1][1][1]
   (add one 'L' after 'L', and at n-1, we've already have one 'A' in our string)

 (initial) dp[1] = [
             [1,1,0], 
//no 'A' in our string: The ending have 0 continuous 'L', which is 'P';
The ending have 1 continuous 'L', which is 'L';
The ending have 2 continuous 'L', which is impossible.

             [1,0,0]
//one 'A' in our string: The ending have 0 continuous 'L', which is 'A';
The ending have 1 continuous 'L', which is impossible;
The ending have 2 continuous 'L', which is impossible.
]

What's more, we notice that dp is only dependent on n-1, so we can use 2-dimensional vector to conduct our dynamic programming.

One thing we mush be aware is that number in our vector will be pretty large!!! So just try to mod M at each summation step.

The C++ code:

 class Solution {
 public:
int checkRecord(int n) {
    const int M = 1000000007;
    vector< vector<long> > dp(2, vector<long>(3, 0));
    dp = {{1,1,0},{1,0,0}};
    for(int i = 1; i < n; ++i){
        vector< vector<long> > tmp(2, vector<long>(3, 0));
        tmp[0][0] = ((dp[0][0] + dp[0][1] + dp[0][2])%M);
        tmp[0][1] = dp[0][0]%M;
        tmp[0][2] = dp[0][1];
        tmp[1][0] = (((dp[0][0] + dp[0][1] + dp[0][2])%M + (dp[1][0] + dp[1][1] + dp[1][2])%M))%M;
        tmp[1][1] = dp[1][0]%M;
        tmp[1][2] = dp[1][1]%M;
        dp = tmp;
    }
    long res = 0;
    for(int A = 0; A < 2; ++A){
        for(int L = 0; L < 3; ++L){
            res += dp[A][L]%M;
        }
    }
    return res%M;
}
};
```



