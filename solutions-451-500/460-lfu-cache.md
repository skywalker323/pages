# 460. LFU Cache

### Problem:

Design and implement a data structure for Least Frequently Used \(LFU\) cache. It should support the following operations: get and put.

get\(key\) - Get the value \(will always be positive\) of the key if the key exists in the cache, otherwise return -1.  
put\(key, value\) - Set or insert the value if the key is not already present. When the cache reaches its capacity, it should invalidate the least frequently used item before inserting a new item. For the purpose of this problem, when there is a tie \(i.e., two or more keys that have the same frequency\), the least recently used key would be evicted.

Follow up:  
Could you do both operations in O\(1\) time complexity?

Example:

```
LFUCache cache = new LFUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.get(3);       // returns 3.
cache.put(4, 4);    // evicts key 1.
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

### Solutions:

```cpp
Based on ideas from this paper http://dhruvbird.com/lfu.pdf.

      Increasing frequencies
  ----------------------------->

+------+    +---+    +---+    +---+
| Head |----| 1 |----| 5 |----| 9 |  Frequencies
+------+    +-+-+    +-+-+    +-+-+
              |        |        |
            +-+-+    +-+-+    +-+-+     |
            |2,3|    |4,3|    |6,2|     |
            +-+-+    +-+-+    +-+-+     | Most recent 
                       |        |       |
                     +-+-+    +-+-+     |
 key,value pairs     |1,2|    |7,9|     |
                     +---+    +---+     v

Similar to bucket sort, we place key,value pairs with the same frequency into the same bucket, within each bucket, the pairs are sorted according to most recent used, i.e., the one that is most recently used (set,get) is at the bottom of each bucket.


I think this is a very interesting solution, thank you very much. Another idea would be to change the list<list<int>> to unordered_map<int, list<int>>: when we invalidate the LFU key-value pair, we immediately insert a new key whose frequency is 1, so we can maintain a minFreq and still be able to find LFU in O(1) time.

Here I implemented your idea in my way (though slightly more complicate):

class LFUCache {
private:
    unordered_map<int, int> valMap;  // key --> value
    unordered_map<int, int> freqMap;  // key --> frequence
    unordered_map<int, list<list<int>>::iterator> headMap;  // frequence --> bucket head
    unordered_map<int, list<int>::iterator> nodeMap;  // key --> node in list
    list<list<int>> buckets;  // buckets

    int cap;
    int num;

    /* increase the frequence of @key */
    void touch(int key) {
        int freq = freqMap[key];
        auto head = headMap[freq];  // head of the bucket containing key
        auto node = nodeMap[key];  // the list node containing key
        head->erase(node);  // delete the key from this bucket
        // find new bucket head
        auto newHead = head;
        if (headMap.find(freq + 1) != headMap.end())
            newHead = headMap[freq + 1];
        else {
            // Note: that's "insert before" for C++
            buckets.insert(head, list<int>());
            newHead--;
        }
        // insert the key into another bucket
        // put the most recent values in the front
        newHead->push_front(key);
        // delete empty bucket (for convenience of finding LFU)
        if (head->empty()) {
            buckets.erase(head);
            headMap.erase(freq);
        }
        // update the maps
        freqMap[key]++;
        headMap[freq + 1] = newHead;
        nodeMap[key] = newHead->begin();
    }

public:
    LFUCache(int capacity) {
        cap = capacity;
        num = 0;
    }

    int get(int key) {
        if (valMap.find(key) == valMap.end()) return -1;
        touch(key);  // update frequence
        return valMap[key];
    }

    void put(int key, int value) {
        if (cap == 0) return;

        // Only update value
        if (valMap.find(key) != valMap.end()) {
            valMap[key] = value;
            touch(key);
            return;
        }
        // Need to evict a LFU key-value pair
        if (num >= cap) {
            // find the head of the bucket of minimum frequency
            auto head = buckets.end();
            head--;
            // find least visited key of this frequency and delete it
            int evict = head->back();
            head->pop_back();
            int freq = freqMap[evict];
            // if this bucket becomes empty, then delete it
            if (head->empty()) {
                headMap.erase(freq);
                buckets.pop_back();
            }
            // delete the evicted key
            valMap.erase(evict);
            freqMap.erase(evict);
            nodeMap.erase(evict);
            num--;
        }

        // find the head of frequency 1
        if (headMap.find(1) == headMap.end()) {
            buckets.push_back(list<int>());
            auto head = buckets.end(); head--;
            headMap[1] = head;
        }
        // insert new key at the front of list
        auto head = headMap[1];
        head->push_front(key);
        nodeMap[key] = head->begin();
        valMap[key] = value;
        freqMap[key] = 1;
        num++;
    }
};
```



