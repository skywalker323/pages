# 251 Flatten 2D Vector

#Problem:

Implement an iterator to flatten a 2d vector.

For example,
Given 2d vector =
```
[
  [1,2],
  [3],
  [4,5,6]
]
```
By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,2,3,4,5,6].

Follow up:
As an added challenge, try to code it using only iterators in C++ or iterators in Java.

# Thoughts:

A straightforward way is to flatten the 2D array into a 1D array in the constructor, then everything is easy. But this is not efficient.

When can use two Integer to represent the current index in the 2D array, and it starts with 0 and 0. We need to update the index when we call next() and hasNext().

Actually by using two Iterator is making the problem much easier.
It’s similar to two Integer index, we might need to update the two Iterator when we call next() and hasNext().
E.g [[1],[],[2,3]].

# Solutions:

 ```java
class Vector2D {
public:
    vector<int> v_;
    vector<int>::iterator it;
    Vector2D(vector<vector<int>>& vec) {
        for(auto v : vec)
        {
            for(auto iv : v)
            {
                v_.push_back(iv);
            }
        }
        it = v_.begin();
    }

    int next() {

       return *it++;
    }

    bool hasNext() {
        return it != v_.end();
    }
};
 ```
