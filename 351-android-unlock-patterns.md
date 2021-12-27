# 351 Android Unlock Patterns

### Problem:

<pre>
Given an Android 3x3 key lock screen and two integers m and n, where 1 ≤ m ≤ n ≤ 9, count the total number of unlock patterns of the Android lock screen, which consist of minimum of m keys and maximum n keys.

Rules for a valid pattern:
Each pattern must connect at least m keys and at most n keys.
All the keys must be distinct.
If the line connecting two consecutive keys in the pattern passes through any other keys, the other keys must have previously selected in the pattern. No jumps through non selected key is allowed.
The order of keys used matters.

![](/assets/android.png)

Explanation:
| 1 | 2 | 3 |
| 4 | 5 | 6 |
| 7 | 8 | 9 |
Invalid move: 4 - 1 - 3 - 6 
Line 1 - 3 passes through key 2 which had not been selected in the pattern.

Invalid move: 4 - 1 - 9 - 2
Line 1 - 9 passes through key 5 which had not been selected in the pattern.

Valid move: 2 - 4 - 1 - 3 - 6
Line 1 - 3 is valid because it passes through key 2, which had been selected in the pattern

Valid move: 6 - 5 - 4 - 1 - 9 - 2
Line 1 - 9 is valid because it passes through key 5, which had been selected in the pattern.

Example:
Given m = 1, n = 1, return 9.
</pre>


### Solutions:

```java
This problem is intimidating at first glance, but once you understand the jumping logic, the problem boils down to a simple DFS.

For building the pattern using numbers 1 - 10 without duplicates we use dfs and keep track of numbers we have already used so that we avoid duplicates. Imagine a tree:

				1
			2 3 4 5 6 7 8 9 used{ 1 }
	3 4 5 6 7 8 9 used{ 1, 2 }    2 4 5 6 7 8 9 used{ 1, 3 } ...
Our base case is when the current pattern length is at n (max length)

Now we just need to consider the logic around jumping from one number to another number. Jumping here means going from one number to the next requires traversal of another number. We initialize a jumps array where:
jumps[from][to] = jumped|undefined
If we are jumping then we make sure the jump is valid by checking that our used set has the number that will be jumped.

Runtime Complexity: O(n!) the upper bound for the number of patterns

let jumps = Array.from(new Array(10), () => new Array(10))

jumps[1][3] = jumps[3][1] = 2;
jumps[4][6] = jumps[6][4] = 5;
jumps[7][9] = jumps[9][7] = 8;
jumps[1][7] = jumps[7][1] = 4;
jumps[2][8] = jumps[8][2] = 5;
jumps[3][9] = jumps[9][3] = 6;
jumps[1][9] = jumps[9][1] = jumps[3][7] = jumps[7][3] = 5;

var numberOfPatterns = function(m, n) {
    let count = 0
    buildPattern()
    return count
    
    function buildPattern(curr = '', used = new Set) {
        if (curr.length >= m) {
            count++
        }
        if (curr.length === n) {
            return
        }
        for (let i = 1; i < 10; i++) {
            if (used.has(i)) continue
            let jumping = jumps[i][curr[curr.length - 1]]
            if (jumping) {
                if (!used.has(jumping)) continue
            }
            used.add(i)
            buildPattern(curr + i, used)
            used.delete(i)
        }
    }
};
```