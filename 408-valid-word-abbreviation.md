# 408. Valid Word Abbreviation

### Problem:

Given a non-empty string s and an abbreviation abbr, return whether the string matches with the given abbreviation.

A string such as "word" contains only the following valid abbreviations:

```
["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]
```

Notice that only the above abbreviations are valid abbreviations of the string "word". Any other string is not a valid abbreviation of "word".

Note:  
Assume s contains only lowercase letters and abbr contains only lowercase letters and digits.

Example 1:

```
Given s = "internationalization", abbr = "i12iz4n":

Return true.
```

Example 2:

```
Given s = "apple", abbr = "a2e":

Return false.
```

### Problem:

```cpp
We need to add bits from RHS, so for that we have to keep a common counter here k which helps us track kth last bit for both strings.

Now for each battle of bits there will be 8 cases (as it depends on A's ith bit, B's ith bit and the carry bit), as stated:
PS: Here res is resultant string

A[i]	B[i]	Carry	res[i]	New Carry
1	1	1	1	1
1	1	0	0	1
1	0	1	0	1
1	0	0	1	0
0	1	1	0	1
0	1	0	1	0
0	0	1	1	0
0	0	0	0	0
From table above we can observe that

res[i] = (a[i] + b[i] + carry) % 2 since it all has to be binary.
But the carry bit will be 1 for all the cases where any two of a[i], b[i] or oldCarryBit is checked.
Simply iterate over both the strings and keep adding the size - k th bit if it exists along with carry.
In the end check if the carry bit still exists append it to the head of res. For cases like:
a -> 111
b -> 1
res -> 1000
C++ Code 👨‍💻:
class Solution {
public:
    string addBinary(string a, string b) {
        int aLen = a.size() - 1, bLen = b.size() - 1, k = 0;
        string res = "";
        bool carry = 0;
        while(k <= aLen || k <= bLen) {
            int val = (k <= aLen && a[aLen-k] == '1') + (k <= bLen && b[bLen-k] == '1') + carry;
            res = to_string(val%2) + res;
            carry = val >= 2;
            k++;
        }
        return (carry ? to_string(carry) : "") + res;
    }
};
```



