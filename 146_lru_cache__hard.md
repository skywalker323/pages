# 146 LRU Cache – Hard

### Problem:

Design and implement a data structure for Least Recently Used \(LRU\) cache. It should support the following operations: get and set.

get\(key\) – Get the value \(will always be positive\) of the key if the key exists in the cache, otherwise return -1.  
set\(key, value\) – Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

### Thoughts:

By using a HashMap, it’s easy to implement normal set and get of the cache.  
The most tricky part is that how to find the least used element.  
The idea here is to use a list to keep the elements so that the least used element is in the right.  
Whenever we get or set with a key, we move the element to the most right of the list so that it will be removed from the list last.

In order to make codes look cleaner, we need to have a head and tail which is dummy node.

### Solutions:

```cpp
	template<typename T>
	struct lru
	{
	    lru(int capacity) : capacity_{capacity} {}
	
	    T get_item(const T& key)
	    {
	        auto it = um_.find(key);
	
	        if( it == um_.end())
	        {
	            throw std::invalid_argument("key does not exist");
	        }
	        keys_.erase(keys_.find(key));
	        keys_.push_front(key);
	        return *it;
	    }
	
	    void add_item(const T& key, const T& value)
	    {
	        auto it = um_.find(key);
	
	        if( it == um_.end())
	        {
	            if( keys_.size() == capacity_)
	            {
	                // evac last elem
	                auto l = keys_.back();
	                keys_.pop_back();
	                um_.erase(um_.find(l));
	            }
	        }
	        else
	        {
	            keys_.erase(keys_.find(key));
	            um_.erase(it);
	        }
	
	        um_.insert({key, value});
	        keys_.push_front(key);
	    }
	private:
	    const int capacity_;
	    unordered_map<T, T> um_;
	    list<T> keys_;
};
```



