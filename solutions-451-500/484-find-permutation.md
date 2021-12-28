# 484. Find Permutation

### Problem:

By now, you are given a secret signature consisting of character 'D' and 'I'. 'D' represents a decreasing relationship between two numbers, 'I' represents an increasing relationship between two numbers. And our secret signature was constructed by a special integer array, which contains uniquely all the different number from 1 to n \(n is the length of the secret signature plus 1\). For example, the secret signature "DI" can be constructed by array \[2,1,3\] or \[3,1,2\], but won't be constructed by array \[3,2,4\] or \[2,1,3,4\], which are both illegal constructing special string that can't represent the "DI" secret signature.

On the other hand, now your job is to find the lexicographically smallest permutation of \[1, 2, ... n\] could refer to the given secret signature in the input.

Example 1:

```
Input: "I"
Output: [1,2]
Explanation: [1,2] is the only legal initial spectial string can construct secret signature "I", where the number 1 and 2 construct an increasing relationship.
```

Example 2:

```
Input: "DI"
Output: [2,1,3]
Explanation: Both [2,1,3] and [3,1,2] can construct the secret signature "DI", 
but since we want to find the one with the smallest lexicographical permutation, you need to output [2,1,3]
```

Note:

1. The input string will only contain the character 'D' and 'I'.
2. The length of input string is a positive integer and will not exceed 10,000

### Solutions:

```java
Idea is pretty simple. There are 2 possibilities:

String starts with "I". Then we should use 1 as the first item.
String starts with "D..DI" (k letters) or the string is just "D...D". In this case we should use k, k - 1, ..., 1
to get lexicographically smallest permutation.
Then we proceed computing the rest of the array. Also we should increase min variable that is used to store minimum 
value in remaining part of the array.

public int[] findPermutation(String s) {
    
    s = s + ".";
    int[] res = new int[s.length()];
    int min = 1, i = 0;

    while (i < res.length) {
        if (s.charAt(i) != 'D') {
            res[i++] = min++;
        } else {
            int j = i;
            while (s.charAt(j) == 'D') j++;
            for (int k = j; k >= i; k--)
                res[k] = min++;
            i = j + 1;
        }
    }

    return res;
}
```



