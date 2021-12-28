# 514 Freedom Trail

### Problem:

In the video game Fallout 4, the quest "Road to Freedom" requires players to reach a metal dial called the "Freedom Trail Ring", and use the dial to spell a specific keyword in order to open the door.

Given a string ring, which represents the code engraved on the outer ring and another string key, which represents the keyword needs to be spelled. You need to find the minimum number of steps in order to spell all the characters in the keyword.

Initially, the first character of the ring is aligned at 12:00 direction. You need to spell all the characters in the string key one by one by rotating the ring clockwise or anticlockwise to make each character of the string key aligned at 12:00 direction and then by pressing the center button.   
At the stage of rotating the ring to spell the key character key\[i\]:  
You can rotate the ring clockwise or anticlockwise one place, which counts as 1 step. The final purpose of the rotation is to align one of the string ring's characters at the 12:00 direction, where this character must equal to the character key\[i\].  
If the character key\[i\] has been aligned at the 12:00 direction, you need to press the center button to spell, which also counts as 1 step. After the pressing, you could begin to spell the next character in the key \(next stage\), otherwise, you've finished all the spelling.  
Example:

![](/assets/ring.jpg)

```
Input: ring = "godding", key = "gd"
Output: 4
Explanation:
 For the first key character 'g', since it is already in place, we just need 1 step to spell this character. 
 For the second key character 'd', we need to rotate the ring "godding" anticlockwise by two steps to make it become "ddinggo".
 Also, we need 1 more step for spelling.
 So the final output is 4.
```

Note:  
Length of both ring and key will be in range 1 to 100.  
There are only lowercase letters in both strings and might be some duplcate characters in both strings.  
It's guaranteed that string key could always be spelled by rotating the string ring.

### Solutions:

```cpp
// Given a string ring:

  0   1   2   3   4   5   6
+---+---+---+---+---+---+---+
| G | O | D | D | I | N | G |
+---+---+---+---+---+---+---+

// and keyword "GDI"

      0   1   2   3   4   5   6
    +---+---+---+---+---+---+---+
    | G | O | D | D | I | N | G |
    +---+---+---+---+---+---+---+
  G | 1 |   |   |   |   |   | 2 |
  D |   |   | 4 | 5 |   |   |   |
  I |   |   |   |   | 7 |   |   |                 

// For the first character G of KEY "GDI", 
// we have two Gs in the string ring, ring[0] and ring[6], 
// Let OPT(KEY) denote the minimum number of steps to spell KEY.
KEY: G
// If we choose G0
OPT(G0) = 0 + 1 = 1
// If we choose G6
OPT(G6) = 1 + 1 = 2

// For the second character D of KEY "GDI", 
// we have two Ds in the string ring, ring[2] and ring[3], 
KEY: GD
// and the ring now points to G (G0 or G6).
// If we choose D2
OPT(GD2) = min(OPT(G0) + G0->D2, OPT(G6) + G6->D2) + 1 = 4
// If we choose D3
OPT(GD3) = min(OPT(G0) + G0->D3, OPT(G6) + G6->D3) + 1 = 5

// For the third character I of KEY "GDI", 
// we have one I in the ring string, ring[4], 
// and the ring now points to D (D2 or D3).
KEY: GDI
OPT(GDI4) = min(OPT(GD2) + D2->I4, OPT(GD3) + D3->I4) + 1 = 7


// Sub-problems and state:
Let dp[i][j] denote the minimum number of steps to spell keyword[0..i] with ring arrow pointing at index j.
// OPT(GD2) => dp[1][2]
// keyword(GD)
//         01
// the dial stops at D
//                   2

// OPT(GDI4) => dp[2][4]
// keyword(GDI)
//         012
// the dial stops at I
//                   4

Recurrence Relation:
// Emm.. Too lazy to figure this. I believe you're able to figure this out by yourself or you can look at my code directly,
// which should be very easy to understand.
Code
class Solution {
public:
    int findRotateSteps(string ring, string key) {
        int N = ring.length();
        int M = key.length();
        
        vector<vector<int>> dp(N, vector<int>(M, INT_MAX));
        
        vector<vector<int>> char_pos_map(26, vector<int>());
        for(int i = 0; i < N; ++i) {
            int cur_char = ring[i] - 'a';
            char_pos_map[cur_char].push_back(i);
        }
        
        int res = INT_MAX;
        for(int i = 0; i < M; ++i){
            int cur_char = key[i] - 'a';
            for(int cur_pos : char_pos_map[cur_char]){
                if(i == 0) {
                    dp[cur_pos][0] = min(cur_pos, N - cur_pos) + 1;
                } else {
                    int pre_char = key[i - 1] - 'a';
                    for(int pre_pos : char_pos_map[pre_char]){
                        int diff = min(abs(cur_pos - pre_pos), N - abs(cur_pos - pre_pos));
                        dp[cur_pos][i] = min(dp[cur_pos][i], dp[pre_pos][i - 1] + diff + 1);
                    }
                }
                if(i == M - 1)
                    res = min(res, dp[cur_pos][M - 1]);
            }
        }

        return res;
    }
};
```



