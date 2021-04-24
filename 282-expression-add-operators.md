# 282. Expression Add Operators

### Problem:

Given a string that contains only digits 0-9 and a target value, return all possibilities to add binary operators (not unary) +, -, or * between the digits so they evaluate to the target value.

Examples:
```
"123", 6 -> ["1+2+3", "1*2*3"]
"232", 8 -> ["2*3+2", "2+3*2"]
"105", 5 -> ["1*0+5","10-5"]
"00", 0 -> ["0+0", "0-0", "0*0"]
"3456237490", 9191 -> []
```

### Solutions:

```cpp
class Solution {
public:

    void p(string c, long sum, long aToSum, vector<string>& r, string num, int t)
    {
        if(num.size() == 0 && t == sum)
        {
            r.push_back(c);
            return;
        }

        for (int i = 1; i <= num.size(); i ++) {
             string first = num.substr(0, i);
             string second = num.substr(i);
            if (first[0] == '0' && first.size() > 1) {
                return;
            }

            long firstLong =stol(first);

            if (c  =="" ) {
                p(first, firstLong, firstLong, r, second, t);
            }
            else {
                p(c + "+" + first, sum + firstLong, firstLong, r, second, t);
                p(c + "-" + first, sum - firstLong, -firstLong, r, second, t);
                p(c + "*" + first, (sum - aToSum) + aToSum * firstLong,
                           aToSum * firstLong, r, second, t);
            }

        }
    }

    vector<string> addOperators(string num, int target) {
        vector<string> results;
        p("", 0, 0, results, num, target);
        return results;
    }
};
```
