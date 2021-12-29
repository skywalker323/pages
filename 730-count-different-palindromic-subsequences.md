# 730 Count Different Palindromic Subsequences

### Problem

Given a string S, find the number of different non-empty palindromic subsequences in S, and return that number modulo 10^9 + 7.

A subsequence of a string S is obtained by deleting 0 or more characters from S.

A sequence is palindromic if it is equal to the sequence reversed.

Two sequences A\_1, A\_2, ... and B\_1, B\_2, ... are different if there is some i for which A\_i != B\_i.

Example 1:

```
Input: 
S = 'bccb'
Output: 6
Explanation: 
The 6 different non-empty palindromic subsequences are 'b', 'c', 'bb', 'cc', 'bcb', 'bccb'.
Note that 'bcb' is counted only once, even though it occurs twice.
```

Example 2:

```
Input: 
S = 'abcdabcdabcdabcdabcdabcdabcdabcddcbadcbadcbadcbadcbadcbadcbadcba'
Output: 104860361
Explanation: 
There are 3104860382 different non-empty palindromic subsequences, which is 104860361 modulo 10^9 + 7.
```

Note:

The length of S will be in the range \[1, 1000\].  
Each character S\[i\] will be in the set {'a', 'b', 'c', 'd'}.

### Solutions

```java
class Solution {
    public int countPalindromicSubsequences(String S) {
        int base = 1000000007;
        long[][] dp = new long[S.length()][S.length()];
        for (int l = 1; l <= S.length(); l ++) {
            for (int i = 0; i + l - 1 < S.length(); i ++) {
                int j = i + l - 1;
                if (l == 1) {
                    dp[i][j] = 1;
                    continue;
                }
                if (l == 2) {
                    dp[i][j] = 2;
                    continue;
                }
                if (S.charAt(i) == S.charAt(j)) {
                    int left = i + 1, right = j - 1;
                    while (left <= right && S.charAt(left) != S.charAt(i)) {
                        left ++;
                    }
                    while (left <= right && S.charAt(right) != S.charAt(i)) {
                        right --;
                    }
                    if (left > right) {
                        dp[i][j] = dp[i + 1][j - 1] * 2 + 2;
                    }
                    else if (left == right) {
                        dp[i][j] = dp[i + 1][j - 1] * 2 + 1;
                    }
                    else {
                        dp[i][j] = dp[i + 1][j - 1] * 2 - dp[left + 1][right - 1];
                    }
                }
                else {
                    dp[i][j] = dp[i][j - 1] + dp[i + 1][j] - dp[i + 1][j - 1] ;
                }
                // dp[i][j] = dp[i][j] % base;
                dp[i][j] = dp[i][j] < 0? dp[i][j] + base:dp[i][j]% base;
            }
        }
        return (int)dp[0][S.length() - 1];
    }
}


Intuition and Algorithm
Not necessarily distinct palindromic
Assume that we are interested counting, not necessarily distinct palindromic subsequences. This is dynamic 
programming (DP) problem.

DP(L, R), is the number of palindromic subsequences in S[L .... R]. Its formulated as following.

0 , if L>R
1 , if L=R
DP(L + 1, R) + DP(L, R-1) - DP(L+1, R-1) , if S[L] != S[R]
DP(L + 1, R) + DP(L, R-1) + 1 , if S[L] = S[R]
Why the third one?
Check the below image, if S[L] != S[R] then DP(L,R) is represented by blue (line) + green (line) - red (line) ,
the last one is because of repeated elements between blue and green lines.

image

Why the fourth one?

First assume that S[L]!= S[R], the formula is: DP(L + 1, R) + DP(L, R-1) - DP(L+1, R-1)
Consider S = a.....b...a...b.....a
Now check when S[L]= S[R], we have to add all palindromes that contains (L+1,R-1) because they are bordered by a 
character + 1 (only the border).
The formula will be: DP(L, R) = (DP(L + 1, R) + DP(L, R-1) - DP(L+1, R-1)) + (DP(L+1, R-1)+1 ), solving that , we have.

DP(L, R) = DP(L + 1, R) + DP(L, R-1) + 1

Distinct palindromic
DP(L, R, alpha), is the number of distinct palindromic subsequences in S[L .... R] bordered by alpha. . 
Its formulated as following.

DP(L, R, alpha), is the number of palindromic subsequences in S[L .... R]. Its formulated as following.

0 , if L>R or (L=R and S[L]!=alpha)
1 , if L=R and S[L]=alpha
DP(L + 1, R, alpha) + DP(L, R-1,alpha) - DP(L+1, R-1,alpha) , if S[L] != S[R]
2+ SUM(DP(L+1,R-1, Betha) ) where Betha is all the alphabet , if S[L] = S[R] and S[L]=alpha
Why the third one?
Similar that , not necessarily distinct palindromic.
Why the fourth one?
Its easy to verify.
S[L......R] = [a.......b......a]
2+ SUM(DP(L+1,R-1, Betha) ), number 2 is because of getting ( a and aa)
the sum is the result of getting palindromes with border a.

C++

int memo[1001][1001][4];
class Solution {
public:
	string S;
	int MOD = 1000000007;

	int dp(int start,int end,int alpha){
		//base case
		if(start>end)return 0;
		if(start==end){
			if(S[start] == (alpha+'a') )return 1;
			return 0;
		}
		
		if(memo[start][end][alpha]!=-1)return memo[start][end][alpha];
		
		int dev=0;
		if(S[start]==S[end] && S[start]==(alpha+'a')){
			dev=2;
			for(int i=0;i<4;i++)
				dev=(dev + dp(start+1,end-1,i) )%MOD;
		}else{
			dev= (dev + dp(start,end-1,alpha))%MOD;
			dev= (dev + dp(start+1,end,alpha))%MOD;
			dev= (dev - dp(start+1,end-1,alpha))%MOD;
			if(dev<0)dev+=MOD;
		}
		
		memo[start][end][alpha]=dev;
		return dev;
	}
	
	int countPalindromicSubsequences(string _S) {
		S=_S;
		memset(memo,-1,sizeof(memo));
		int ans=0;
		
		for(int i=0;i<4;i++)
			ans= (ans + dp(0, S.size()-1, i))%MOD;
		
		return ans;        
	}
};
```



