# 503 Next Greater Element II

### Problem:

Given a circular array \(the next element of the last element is the first element of the array\), print the Next Greater Number for every element. The Next Greater Number of a number x is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, output -1 for this number.

Example 1:

```
Input: [1,2,1]
Output: [2,-1,2]
Explanation: The first 1's next greater number is 2; 
The number 2 can't find next greater number; 
The second 1's next greater number needs to search circularly, which is also 2.
```

Note: The length of given array won't exceed 10000.

### Solutions:

```java
Imagine the input array as a concatenation of the same array, twice. [1,2,1] -> [1,2,1,1,2,1]
Similar to Next Greater Element, store the index in the stack instead of the actual value.
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) 
    {
        int n = nums.size();
        nums.resize(2*n);
        
        for(int i=n; i<2*n; i++) //concatenate the same array
        {
            nums[i] = nums[i-n];
        }
        
        vector<int> res(n, -1); //to be returned, initialize it with -1
        stack<int> st;
        
        for(int i=0; i<2*n; i++)
        {
            int ele = nums[i];
            
            while(!st.empty() && ele > nums[st.top()])
            {
				//ele acts as NGE to the value at st.top()
				
                if(st.top() >= n) //index should not exceed n
                {
                    st.top() = st.top() - n;
                }
                
                res[st.top()] = ele;
                st.pop();
            }
            
            st.push(i);
        }
        
        return res;
    }
};
```



