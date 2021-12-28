# 432. All O\`one Data Structure

### Problem

Implement a data structure supporting the following operations:

Inc\(Key\) - Inserts a new key with value 1. Or increments an existing key by 1. Key is guaranteed to be a non-empty string.  
Dec\(Key\) - If Key's value is 1, remove it from the data structure. Otherwise decrements an existing key by 1. If the key does not exist, this function does nothing. Key is guaranteed to be a non-empty string.  
GetMaxKey\(\) - Returns one of the keys with maximal value. If no element exists, return an empty string "".  
GetMinKey\(\) - Returns one of the keys with minimal value. If no element exists, return an empty string "".  
Challenge: Perform all these in O\(1\) time complexity.

### Solutions

```cpp
For each value, I have a bucket with all keys which have that value. 
The buckets are in a list, sorted by value. That allows constant time insertion/erasure and 
iteration to the next higher/lower value bucket. A bucket stores its keys in a hash set for easy
 constant time insertion/erasure/check (see first two posts here if you're worried). 
 I also have one hash map to look up which bucket a given key is in.

Based on a previously flawed Python attempt (I just couldn't find a good way to get an 
arbitrary element from a set) but also influenced by an earlier version of @Ren.W's solution. 
We ended up with quite similiar code, I guess there's not much room for creativity once you decide on 
the data types to hold the data.

class AllOne {
public:

    void inc(string key) {

// If the key doesn't exist, insert it with value 0.
        if (!bucketOfKey.count(key))
            bucketOfKey[key] = buckets.insert(buckets.begin(), {0, {key}});

// Insert the key in next bucket and update the lookup.
        auto next = bucketOfKey[key], bucket = next++;
        if (next == buckets.end() || next->value > bucket->value + 1)
            next = buckets.insert(next, {bucket->value + 1, {}});
        next->keys.insert(key);
        bucketOfKey[key] = next;

        // Remove the key from its old bucket.
        bucket->keys.erase(key);
        if (bucket->keys.empty())
            buckets.erase(bucket);
    }

    void dec(string key) {

// If the key doesn't exist, just leave.
        if (!bucketOfKey.count(key))
            return;

// Maybe insert the key in previous bucket and update the lookup.
        auto prev = bucketOfKey[key], bucket = prev--;
        bucketOfKey.erase(key);
        if (bucket->value > 1) {
            if (bucket == buckets.begin() || prev->value < bucket->value - 1)
                prev = buckets.insert(bucket, {bucket->value - 1, {}});
            prev->keys.insert(key);
            bucketOfKey[key] = prev;
        }

// Remove the key from its old bucket.
        bucket->keys.erase(key);
        if (bucket->keys.empty())
            buckets.erase(bucket);
    }

    string getMaxKey() {
        return buckets.empty() ? "" : *(buckets.rbegin()->keys.begin());
    }

    string getMinKey() {
        return buckets.empty() ? "" : *(buckets.begin()->keys.begin());
    }

private:
    struct Bucket { int value; unordered_set<string> keys; };
    list<Bucket> buckets;
    unordered_map<string, list<Bucket>::iterator> bucketOfKey;
};
```



