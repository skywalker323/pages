# 670 Maximum Swap

### Problem

Given a non-negative integer, you could swap two digits at most once to get the maximum valued number. Return the maximum valued number you could get.

Example 1:

```
Input: 2736
Output: 7236
Explanation: Swap the number 2 and the number 7.
```

Example 2:

```
Input: 9973
Output: 9973
Explanation: No swap.
```

Note:  
The given number is in the range \[0, 108\]

### Solutions

```java
Actually this problem can be easily solved by only one pass from backward. During the scan, we only need to do 2 
things:

record the largest digit (maxdigit) and its corresponding index (maxidx);
if the current digit is smaller than the largest digit, this digit and the largest digit are the best candidate for
 max swap so far. In this case, this digit pair is recorded (leftidx and rightidx).

    int maximumSwap(int num) {
        string numstr = std::to_string(num);

        int maxidx = -1; int maxdigit = -1;
        int leftidx = -1; int rightidx = -1;        

        for (int i = numstr.size() - 1; i >= 0; --i) {
            // current digit is the largest by far
            if (numstr[i] > maxdigit) {
                maxdigit = numstr[i];
                maxidx = i;
                continue;
            }

            // best candidate for max swap if there is no more 
            // such situation on the left side
            if (numstr[i] < maxdigit) {
                leftidx = i;
                rightidx = maxidx;
            }
        }

        // num is already in order
        if (leftidx == -1) return num;

        std::swap(numstr[leftidx], numstr[rightidx]);

        return std::stoi(numstr);
    }
```



