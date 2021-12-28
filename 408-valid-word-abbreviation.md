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
The idea is straightforward: simply scan the abbreviation string a and word w backward. While scanning a:

if encounter a letter c, check against the current char in w.
if encounter a digit c, skip (c-'0')*multi many chars in w, where multi = 10^(k-1) for the kth
 consecutive digit char backward. Also, if c == '0', make sure it is not a leading 0
  (this is a requirement from the problem).
The idea to use backward scan instead of forward scan is to easily count how many 
chars in w needs to be skipped while scanning only the current char in a and no need 
to form any int of multi-digit numbers.

Reverse index scan:

bool validWordAbbreviation(string w, string a) {
      int iw = w.length()-1; double multi = 0.1;
      for (int ia = a.length()-1; ia >= 0; --ia, --iw)
        if (!isdigit(a[ia])) { multi = 0.1; if (a[ia] != w[iw]) return false; }
        else if (a[ia] == '0' && (!ia || !isdigit(a[ia-1]))) return false; 
        else if ((iw -= ((int)(multi*=10)*(a[ia] - '0')-1)) < 0) return false; 
      return iw == -1;
}
Equivalently, you could also use reverse_iterator:

    bool validWordAbbreviation(string w, string a) {
      auto iw = w.rbegin(); double multi = 0.1;
      for (auto ia = a.rbegin(); ia != a.rend(); ++ia, ++iw)
        if (!isdigit(*ia)) { multi = 0.1; if (*ia != *iw) return false; }
        else if (*ia == '0' && (ia+1 == a.rend() || !isdigit(*(ia+1)))) return false; 
        else if ((iw += ((int)(multi*=10)*(*ia - '0')-1)) >= w.rend()) return false;
      return iw == w.rend();
    }
```



