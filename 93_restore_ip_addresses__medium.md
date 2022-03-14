# 93 Restore IP Addresses – Medium

### Problem:

Given a string containing only digits, restore it by returning all possible valid IP address combinations.

For example:  
Given "25525511135",

return \["255.255.11.135", "255.255.111.35"\]. \(Order does not matter\)

### Thoughts:

Modified DFS. Keep a variable to keep current potential solution – String.

Keep how many parts to go.

For each visit, there are three possibilities to go deep level. x, xx, xxx.

Only go deep when the possibility is valid.

### Solutions:

```java
    vector<string> res;
    bool valid_seg(string s)
    {
        return s[0] != '0' ? stoi(s) <=255 : (s.size()==1);        
    }

    void bt(int rem, string temp, string src, int id)
    {
        if( rem == 0 ){
            if( id == src.size() ) res.push_back(temp);
            return; 
        }

        for(int i =1; i <= 3; i++) {
            if( id + i  > src.size()) break;
            auto t = src.substr(id, i);
            if(valid_seg(t)) bt(rem-1, temp==""? t : temp+"."+t, src, id+i);
        }
    }
    vector<string> restoreIpAddresses(string s) {

        bt(4, "", s, 0);
        return res;
    }
```



