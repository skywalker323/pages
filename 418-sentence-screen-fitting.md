# 418 Sentence Screen Fitting

### Problem:

Given a rows x cols screen and a sentence represented by a list of non-empty words, find how many times the given sentence can be fitted on the screen.

Note:

A word cannot be split into two lines.  
The order of words in the sentence must remain unchanged.  
Two consecutive words in a line must be separated by a single space.  
Total words in the sentence won't exceed 100.  
Length of each word is greater than 0 and won't exceed 10.  
1 ≤ rows, cols ≤ 20,000.  
Example 1:

```
Input:
rows = 2, cols = 8, sentence = ["hello", "world"]

Output: 
1

Explanation:
hello---
world---

The character '-' signifies an empty space on the screen.
```

Example 2:

```
Input:
rows = 3, cols = 6, sentence = ["a", "bcd", "e"]

Output: 
2

Explanation:
a-bcd- 
e-a---
bcd-e-

The character '-' signifies an empty space on the screen.
```

Example 3:

```
Input:
rows = 4, cols = 5, sentence = ["I", "had", "apple", "pie"]

Output: 
1

Explanation:
I-had
apple
pie-I
had--

The character '-' signifies an empty space on the screen.
```

### Solutions:

```java
I got this idea from this solution
It took me some time to understand the idea by reviewing the code, i will add more details here, and 
hopefully it helps others to understand easier.
The idea behind this implementation is summarized below:

If we know that how many words will be covered in a row starting each of the given words.
We can iterate through the rows and by starting the very beginning word try to count the number of times 
we can fit the complete sentence in the screen.
The extra memory (used memorization) called dp in the code is what discussed in number 1. In other words, d[i]
 shows if we start filling a row from ith word in the sentence, what will be the index of next word for the
  following row.

Here is small example (value of rows is not needed for this discussion, it is needed for item 2 discussed above):
The last two lines in following example are very important.
It basically reminds that once we reach the end of sentence we start to filling the columns from the beginning 
of the sentence.


{"ab", "cde", "fg", "j"}, rows=3, cols =6

|ab-cde|  --> dp[0] = 2;    // if the starting word for filling a row is "ab"
|cde-fg|  --> dp[1] = 3;    // if the starting word for filling a row is "cde"
|fg-j--|  --> dp[2] =0;     // if the starting word for filling a row is "fg"
|j-ab--|  --> dp[3] = 1;    // if the starting word for filling a row is "j"
After building dp for all the words

Now we need to start iterating through the rows.
Starting from the begining word and we try to keep track of a counter which can be increamented everytime we
cover the complete sentence.
agian notice in the code dp[k] shows the index of the next word whicch should be picked for the new row.

/* This code is borrowed from the following link. It is originally posted by @wonderwddi
(https://leetcode.com/problems/sentence-screen-fitting/discuss/913264/c%2B%2B-with-dp) */

    int wordsTyping(vector<string>& sentence, int rows, int cols) {    
        int num = sentence.size();
        vector<int> dp (num, 0);
        int ret = 0;

        for (int i = 0; i < num; i++) {
            int len = 0;
            int cnt = 0;
            int start = i;

            while (len <= cols) {
                len += sentence[start].size();
                start = (start + 1) % num;
                cnt++;
                if (len < cols) /*for ectar space between words*/
                    len++;
            }
            dp[i] = cnt - 1; /*cnt shows how many words are covered, but since we need to use as index in sentence needed to be decremented*/
        }

        for (int i = 0, k = 0; i < rows; i++) {
            ret += dp[k]; /*this keep track the number of covered words*/
            k = (k + dp[k]) % num;
        }

        return ret/num;
    }
```

```java
Approach-1 (Brute force)
In this approach we try to fill the screen and count the number of times the sentence is fully shown on the screen.
Important notes:
We need to notice some key points:

Each word should be completely presented in each row. In other words it is not allowed to show some characters of a 
word in row1 and the rest in row2.
We need to consider one extra character for having space among words after each character unless:
The word is ended in last column. (i.e assume col size is 10 and we have {"abcd", "efghi"}, in this case the last 
character of the second word will be in 10th column of row i) ----> |abcd-efghi|
The last word is added and we do not have enough remaining space to show all the words in the sentence.
Algorithm:

First try to find the required spaces to put all the words of the given sentence once. The space after the last 
character is removed from this calculation. This will be used when we want to judge whether the extra space after
 picked word is needed or not.
Define a loop that iterates through e rows.
Try to fill each row as much as possible until we reach a point that the remaining space is enough to fill the 
complete sentence.
Move forward and keep track of remaining characters (initialized by col) in each row
Also once we are done with covering all the words of the sentence update the counter.
The following code implements the discussing idea. IF you uncomment the cout lines, it shows similar output as 
discussed in problem description.
class Solution {
public:
    int wordsTyping(vector<string>& sentence, int rows, int cols) {
        int r=0;
        int remain = cols;
        int ret=0;
        int spaceNeeded=0;
        for(int i =0; i<sentence.size()-1; ++i) {            
            spaceNeeded+= sentence[i].length()+1;
        }
        spaceNeeded += sentence[sentence.size()-1].length();

        while (r<rows){
            int i=0;
            for (; i< sentence.size(); ++i) {           
                string str= sentence[i];
        /*addSpace will be 0 or 1, this logic check whether we need to consider space at
         the end of picked word or not*/
                int addSpace = ((i==sentence.size()-1) && (spaceNeeded > remain - str.length()+ (rows-r)*cols) ||
                             remain==str.length())?0:1;
                if(remain>= str.length()+ addSpace){
                    //cout <<str.substr(0,str.length()) << "-";
                    remain = remain - str.length()-addSpace;
                    continue;
                } else { /*moving to a new row*/
                    //cout<<endl;                
                    remain = cols;
                     /*resset the remaining space in each row */    
                    i--; 
                     /*since we need to revisit the last word again since we could not fit it in previous row*/
                    r++;
                    if(r>=rows)
                        break;
                }
            }

            if(i== sentence.size()) 
            /*update the counter only and only if we covered all the words.*/
                ret++;
        }

        return ret;
    }
};
```



