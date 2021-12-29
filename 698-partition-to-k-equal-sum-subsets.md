# 698 Partition to K Equal Sum Subsets

### Problem

Given an array of integers nums and a positive integer k, find whether it's possible to divide this array into k non-empty subsets whose sums are all equal.

Example 1:

```
Input: nums = [4, 3, 2, 3, 5, 2, 1], k = 4
Output: True
Explanation: It's possible to divide it into 4 subsets (5), (1, 4), (2,3), (2,3) with equal sums.
```

Note:

1 &lt;= k &lt;= len\(nums\) &lt;= 16.  
0 &lt; nums\[i\] &lt; 10000.

### Solutions

```java
Put n items into k bucket so each bucket has same total item value.

For each bucket, try all possible content O(k*2^n) -- no good.
For each item, try all possible destined bucket O(n^k) -- doable.
class Solution {
public:
   // use global variables to avoid long parameter list
   int target; // of each bucket
   vector< int > ns;
   vector< int > bucket;

   bool canPartitionKSubsets( vector<int>& nums, int k ) {
       int sum = 0;
       for( int &n : nums ) sum += n;
       if( sum % k ) return false; // not divisible
       target = sum / k;
       ns = vector< int >( nums );
       bucket = vector< int >( k, 0 );
       // starting with bigger ones makes it faster
       sort( ns.begin(), ns.end() );
       reverse( ns.begin(), ns.end() );
       return put( 0 );
   }

   // put #n item of ns into some bucket to meet target
   bool put( int n ) {
       for( int i = 0; i < bucket.size(); ++i ) {
           if( bucket[i] + ns[n] > target ) continue; // try next bucket
           bucket[i] += ns[n]; // put it in!
           if( n == ns.size() - 1 ) return true; // all items in bucket, no overflow
           if( put( n + 1 ) ) return true; // move on to next item
           else { // no solution = wrong bucket
               bucket[i] -= ns[n]; // take it out
               if( bucket[i] == 0 ) return false; // no need to try other empty bucket
           }
       }
       return false; // no bucket fits
   }
};
```

```java
class Solution {
public:
//currsum is the current sum of subset we are working on and targetsum is the sum we require for each subset

    bool dfs(vector<int>& nums,vector<int>visited,int idx,int k,int currsum,int targetsum)
    {
        if(k==1) return true;                                               
        //if k==1 then all the subsets have been found so return true.
        if(currsum==targetsum) return dfs(nums,visited,0,k-1,0,targetsum);  
        //this condition means you have found one suset so start from begining for another subset.
        for(int i=idx ; i<nums.size() ; i++)
        {
            if(!visited[i])                                                
            //if the index is not visited then it can be used in the current subset or bucket.
            {
                visited[i]=true;                                                          
                //set this index as used to avoid redundancy.
                if(dfs(nums,visited,i+1,k,currsum+nums[i],targetsum)) return true;        
                //explore the choices
                visited[i]=false;                                                         
                //for backtrack.
            }
        }
        return false;
    }
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        vector<int>visited(nums.size(),false);
        int sum=0;
        for(auto x:nums) sum+=x;
        if(sum%k!=0) return false;
        int targetsum=sum/k;
        return dfs(nums,visited,0,k,0,targetsum);

    }
};



The best solution for this problem is definitely bit manipulation DP with complexity O(n2^n). Whereas dfs will render a expotential complexity and it is hard to analyze.

Here is my idea: We use mask to determine the state, the state will tell you "which numbers are selected already". For example nums = [2,1,4,3,5,6,2], mask = "1100101" , then that means we have already chosen 2,1,5,2.

Now, how do we consider the conversion between states? Consider the example above, it is clear that we have 4,3,6 unchosen so we can choose any of them. But, as explained in the leetcode official solution, we can't choose a number such that it crosses our target. And dp[mask] will represent sum(chosen number) % target. Hence the state conversion will become:

dp[mask|(1<<i)] = (dp[mask]+nums[i]) % tar;

An Example will be: nums = [4, 3, 2, 3, 5, 2, 1], k = 4, tar = 5
dp["1100101"] represents we have chosen 4,3,5,1, the sum is 4+3+5+1 = 13, 13%5 = 3, hence dp["1100101"] = 3

If dp["11111...1111"] == 0 then that means we can find the solution.

class Solution {
public:
    int dp[(1<<16)+2];
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        int n = nums.size(), sum = 0;
        fill(dp, dp+(1<<16)+2, -1);
        dp[0] = 0;
        for (int i = 0; i < n; i++) sum += nums[i];
        if (sum % k) return false;
        int tar = sum/k;
        
        for (int mask = 0; mask < (1<<n); mask++) {
            if (dp[mask] == -1) continue;  // if current state is illegal, simply ignore it
            for (int i = 0; i < n; i++) {
                if (!(mask&(1<<i)) && dp[mask]+nums[i] <= tar) {  // if nums[i] is unchosen && choose nums[i] would not cross the target
                    dp[mask|(1<<i)] = (dp[mask]+nums[i]) % tar;
                }
            }
        }
        return dp[(1<<n)-1] == 0;
    }
};
```



