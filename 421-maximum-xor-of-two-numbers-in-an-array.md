# 421. Maximum XOR of Two Numbers in an Array

### Problem:

Given a non-empty array of numbers, a0, a1, a2, … , an-1, where 0 ≤ ai &lt; 231.

Find the maximum result of ai XOR aj, where 0 ≤ i, j &lt; n.

Could you do this in O\(n\) runtime?

Example:

```
Input: [3, 10, 5, 25, 2, 8]

Output: 28

Explanation: The maximum result is 5 ^ 25 = 28.
```

### Solutions:

```java
int findMaximumXOR(vector<int>& nums) {
        int n = nums.size();
        
        if (n == 0 || n == 1)
            return 0;
        if (n == 2)
            return nums.at(0) ^ nums.at(1);
        
        list<int> set0;
        list<int> set1;
        int i;
        int j;
        int maxValue;
        
        for (i = 30; i >= 0; i--) {
            for (j = 0; j < n; j++) {
                if ((nums.at(j) & (1<<i)) == 0)
                    set0.push_back(nums.at(j));
                else
                    set1.push_back(nums.at(j));
            }
            
            if (set0.size() != 0 && set1.size() != 0) {
                maxValue = pow(2, i);
                break;
            }
            else {
                set0.clear();
                set1.clear();
            }
        }
        
        if (i == -1)
            return 0;
        
        maxValue += getMaxXor(set0, set1, i-1);
        
        return maxValue;
}

int getMaxXor(list<int>& set0, list<int>& set1, int pos) {
        int maxValue;
        list<int> set0list0;
        list<int> set0list1;
        list<int> set1list0;
        list<int> set1list1;
        int i;
        list<int>::iterator it;
        
        if (set0.size() == 0 || set1.size() == 0 || pos < 0)
            return 0;
        
        for (it = set0.begin(); it != set0.end(); it++) {
            int value = *it;
            if ((value & (1<<pos)) == 0)
                set0list0.push_back(value);
            else
                set0list1.push_back(value);
        }
        
        for (it = set1.begin(); it != set1.end(); it++) {
            int value = *it;
            if ((value & (1<<pos)) == 0)
                set1list0.push_back(value);
            else
                set1list1.push_back(value);
        }
        
        if (set0list0.size() == 0 && set1list0.size() == 0)
            maxValue = getMaxXor(set0, set1, pos-1);
        else if (set0list1.size() == 0 && set1list1.size() == 0)
            maxValue = getMaxXor(set0, set1, pos-1);
        else {
            int maxValue1 = getMaxXor(set0list0, set1list1, pos-1);
            int maxValue2 = getMaxXor(set0list1, set1list0, pos-1);
            maxValue = pow(2, pos) + (maxValue1 > maxValue2 ? maxValue1 : maxValue2);
        }
        
        return maxValue;
 }
Example input: [42, 5, 69, 22, 23, 8, 1, 17, 30, 75, 99]

The max XOR value is (30 ^ 99) = 125. Below are the binary represntations of each number.

42 = 0 1 0 1 0 1 0

5 = 0 0 0 0 1 0 1

69 = 1 0 0 0 1 0 1

22 = 0 0 1 0 1 1 0

23 = 0 0 1 0 1 1 1

8 = 0 0 0 1 0 0 0

1 = 0 0 0 0 0 0 1

17 = 0 0 1 0 0 0 1

30 = 0 0 1 1 1 1 0

75 = 1 0 0 1 0 1 1

99 = 1 1 0 0 0 1 1

Each number in the array is >= 0 and < pow(2,31). So, any number in the array can be represented by atmost 31 bits (bits[30:0]).
The for loop in findMaximumXOR (lines 17-33) finds the max bit position where some of the numbers have '0' in that bit position and the other numbers have '1' in that bit position.
The numbers that have '0' in that bit position go to set0 and the numbers that have '1' in that bit position go to set1. So, for the example above, we have:
set0 = {42, 5, 22, 23, 8, 1, 17,30}
set1 = {69, 75, 99}
Bit position = 6.
The value obtained by XORing bit 6 between set0 and set1 (forgetting the low order bits) is pow(2, 6) = 64.

Next, findMaximumXOR() calls getMaxXor() passing set0 and set1 to recursively find the max value of the XOR between numbers in set0 and set1 for the remaning low order bits. The max XOR value would then be 64 + (the max value returned by getMaxXor()).

The first call would be getMaxXor({42, 5, 22, 23, 8, 1, 17,30}, {69, 75, 99}, 5).
What getMaximumXOR() does is for bit position 5, it splits set0 into two sets - the numbers that have '0' in bit position 5 go to set0list0. The numbers that have '1' in bit position 5 goto set0list1. The same is done for set1 i.e. set1 is also split into two sets set1list0 and set1list1. For this specific example, we have:

set0list0 = {5, 22, 23, 8, 1, 17, 30}
set0list1 = {42}
set1list0 = {69, 75}
set1list1 = {99}

Since at bit position 5 we found set0list0 and set1list1, that means XORing bit 5 (forgetting the low order bits) of numbers between set0list0 and set1list1 woul give pow(2, 5) = 32.
So, the maximum XOR value between numbers in set0list0 and set1list1 would be 32 + (set0list0, set1list1, 4).
Arguing on the same lines, the maximum XOR value between numbers in set0list1 and set1list0 would be 32 + (set0list1, set1list0, 4).
Note that we pair the "opposite" resulting sets to get to the max XOR value i.e. (set0list0, set1list1) and (set0list1, set1list0).

For this specific example the recursive calls would be:

getMaxXor({5, 22, 23, 8, 1, 17, 30}, {99}, 4);
set0list0 = {5, 8, 1}
set0list1 = {22, 23, 17, 30}
set1list0 = {99}
set1list1 = {}

getMaxXor({5, 8, 1}, {}, 3); ==> This call returns 0 as set1 is empty.

getMaxXor({22, 23, 17, 30}, {99}, 3);
set0list0 = {22, 23, 17}
set0list1 = {30}
set1list0 = {99}
set1list1 = {}

getMaxXor({22, 23, 17}, {}, 2); ==> This call returns 0 as set1 is empty.

getMaxXor({30}, {99}, 2);
set0list0 = {}
set0list1 = {30}
set1list0 = {99}
set1list1 = {}

getMaxXor{{30}, {99}, 1};
set0list0 = {}
set0list1 = {30}
set1list0 = {}
set1list1 = {99}

Since both set0list0 and set1list0 are empty, we simply ignore this bit position and check the next lower bit position. So:

getMaxXor({30}, {99}, 0);

set0list0 = {30}
set0list1 = {}
set1list0 = {}
set1list1 = {99}

getMaxXor({30}, {99}, -1) ==>This call would return 0, as bit position is invalid.

getMaxXor({42}, {69, 75}, 4);
set0list0 = {42}
set0list1 = {}
set1list0 = {69, 75}
set1list1 = {}

getMaxXor({42}, {69, 75}, 3);

set0list0 = {}
set0list1 = {42}
set1list0 = {69}
set1list1 = {75}

getMaxXor({}, {75}, 2); ==> This call returns 0 as set0 is empty

getMaxXor({42}, {75}, 2)
set0list0 = {42}
set0list1 = {}
set1list0 = {75}
set1list1 = {}

getMaxXor({42}, {75}, 1)
set0list0 = {}
set0list1 = {42}
set1list0 = {}
set1list1 = {75}

getMaxXor({42}, {75}, 0)
set0list0 = {42}
set0list1 = {}
set1list0 = {}
set1list1 = {75}

getMaxXor({42}, {75}, -1)

In the recursive calls above, the max value is returned by the path:
getMaxXor({42, 5, 22, 23, 8, 1, 17,30}, {69, 75, 99}, 5) //maxValue = 32
getMaxXor({5, 22, 23, 8, 1, 17, 30}, {99}, 4) //maxValue = 16
getMaxXor({22, 23, 17, 30}, {99}, 3) //maxValue = 8
getMaxXor({30}, {99}, 2) //maxValue = 4
getMaxXor{{30}, {99}, 1} //maxValue = 0
getMaxXor({30}, {99}, 0) //maxValue = 1
getMaxXor({30}, {99}, -1) //maxValue = 0

So, the final max XOR value returned = 64 + 32 + 16 + 8 + 4 + 1 = 125.

The run time is O(n) because we iterate 31 times, once for each bit position (i.e. bits 30 to 0). During each bit position, we check/visit each number at most once. So, the time complexity would be 31xn or O(n).
```



