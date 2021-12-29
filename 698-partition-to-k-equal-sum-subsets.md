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
```



