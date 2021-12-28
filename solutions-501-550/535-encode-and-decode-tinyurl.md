# 535 Encode and Decode TinyURL

### Problem:

TinyURL is a URL shortening service where you enter a URL such as [https://leetcode.com/problems/design-tinyurl](https://leetcode.com/problems/design-tinyurl) and it returns a short URL such as [http://tinyurl.com/4e9iAk](http://tinyurl.com/4e9iAk).

Design the encode and decode methods for the TinyURL service. There is no restriction on how your encode/decode algorithm should work. You just need to ensure that a URL can be encoded to a tiny URL and the tiny URL can be decoded to the original URL.

Show Company Tags  
Show Tags  
Show Similar Problems

### Solutions:

```java
class Solution {
public:
    // Encodes a URL to a shortened URL.
    string encode(string longUrl) {
        long_to_short[longUrl] = "http://tinyurl.com/" + to_string(hash_function(longUrl));
        short_to_long[long_to_short[longUrl]] = longUrl;
        return long_to_short[longUrl];
    }

    // Decodes a shortened URL to its original URL.
    string decode(string shortUrl) {
        return short_to_long[shortUrl];
    }
    
private:
    uint64_t hash_function(const string& str) {
        uint64_t hash = 0x811c9dc5;
        uint64_t prime = 0x1000193;

        for(int i = 0; i < str.size(); ++i) {
            uint8_t value = str[i];
            hash = hash ^ value;
            hash *= prime;
        }

        return hash;
    }
    
    unordered_map<string, string> long_to_short;
    unordered_map<string, string> short_to_long;
};
```

```java
public class Codec {
    HashMap<String, String> longToShort = new HashMap<String, String>();
    HashMap<String, String> shortToLong = new HashMap<String, String>();
    String seed = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    int nextid = 0;
    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        if (longToShort.containsKey(longUrl)) {
            return longToShort.get(longUrl);
        }
        StringBuilder sb = new StringBuilder();
        int id = nextid;
        for (int i = 0; i < 6; i ++) {
            int j = id % 62;
            id = id / 62;
            sb.insert(0, seed.charAt(j));
        }
        String res = sb.toString();
        shortToLong.put(res, longUrl);
        longToShort.put(longUrl, res);
        nextid ++;
        return res;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        if (!shortToLong.containsKey(shortUrl)) {
            return null;
        }
        return shortToLong.get(shortUrl);
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(url));
```



