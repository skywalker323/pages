# 525 Contiguous Array

Given a binary array, find the maximum length of a contiguous subarray with equal number of 0 and 1.

Example 1:

```
Input: [0,1]
Output: 2
```

Explanation: \[0, 1\] is the longest contiguous subarray with equal number of 0 and 1.  
Example 2:

```
Input: [0,1,0]
Output: 2
```

Explanation: \[0, 1\] \(or \[1, 0\]\) is a longest contiguous subarray with equal number of 0 and 1.  
Note: The length of the given binary array will not exceed 50,000.

### Solutions

```java
	
	// let's have a counter to count the difference between # of 1 and 0
	// the idea is that when we encounter 1, count++, when we encounter 0 count--
	// when we found that the current count equals to one of  previous counts
	// that means the subarray in between has the same number of 1 and 0
	// Example: 1110 1100 1100
	// for 1110,  the diff between # of 1 and # of 0 is 3-1=2
	// store this in our hashmap Map[2]=>3, where 3 is the current index
	// when we get index 7 we have 1110 1100
	// the diff now is 5-3=2, same as previous count, this means 1100 is balanced
	// update result
	
	// Note that we only need to store count once when count not in HashMap
	// because that index is far left to form possible longest result
		unordered_map<int,int> Map; // count=> index
		Map[0]=-1;
		int count=0;
		int ans=0;
		for(int i=0;i<nums.size();i++)
		{
			if(nums[i]==1)
			{
				count++;
			}
			else
			{
				count--;
			}
			if(Map.count(count))
			{
				ans=max(ans,i-Map[count]);
			}
			else
			{
				Map[count]=i;
			}
		}

		return ans;
	}
```



