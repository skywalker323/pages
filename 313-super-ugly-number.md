# 313 Super Ugly Number

### Problem:

Write a program to find the nth super ugly number.

Super ugly numbers are positive numbers whose all prime factors are in the given prime list primes of size k. For example, \[1, 2, 4, 7, 8, 13, 14, 16, 19, 26, 28, 32\] is the sequence of the first 12 super ugly numbers given primes = \[2, 7, 13, 19\] of size 4.

Note:  
\(1\) 1 is a super ugly number for any given primes.  
\(2\) The given numbers in primes are in ascending order.  
\(3\) 0 &lt; k ≤ 100, 0 &lt; n ≤ 106, 0 &lt; primes\[i\] &lt; 1000.  
\(4\) The nth super ugly number is guaranteed to fit in a 32-bit signed integer.

### Solutions:

```java
class Solution {
    public:
        using P = pair<long long, int>;
        int nthSuperUglyNumber(int n, vector<int>& primes) {
            vector<int> index(primes.size(), 0);
            vector<long long> nums;
            nums.push_back(1);      
            
            priority_queue<P, vector<P>, greater<P>> pq;    
            // remember pair(the ugly number, who generated this ugly)
            for (int i = 0; i < primes.size(); i++)
                pq.emplace(primes[i], i);   // initial ugly = prime  * 1
            
            while (nums.size() < n)
            {
                auto [value, id] = pq.top();    // get the smallest ugly
                pq.pop();
                
                if (value != nums.back())       // check the dupe
                    nums.push_back(value);
                
                index[id]++;                            
                // the prime generated this small ugly number should move forward to multiply next ugly number
                pq.emplace(nums[index[id]] * primes[id], id);         
                // push the new ugly number to pq to participate sorting
            }
            
            return nums.back();
        }
    };
```

```cpp
#define ll long long
class Solution {
    struct triplet{
        ll primeNumber, index, element;
        triplet(ll pn, ll idx, ll e){
            primeNumber = pn;
            index = idx;
            element = e;
        }
    };
    
    struct compare{
        bool operator()(triplet &t1, triplet &t2){
            return t1.element > t2.element;      
        }
    };
    
public:
    int nthSuperUglyNumber(int n, vector<int>& primes) {
        vector<int>v(1,1);
        priority_queue<triplet, vector<triplet>, compare>pq;
        
        for(auto k:primes){
            pq.push({k, 0, k});
        }

        while(v.size() < n){
            triplet top = pq.top();
            pq.pop();
            if(top.element > v.back()){
                v.push_back(top.element);
            }
            pq.push({top.primeNumber, top.index+1, (top.primeNumber * v[(top.index + 1)])});
        }
        
        return v.back();
    }
};
```



