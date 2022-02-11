# 735 Asteroid Collision

### Problem

We are given an array asteroids of integers representing asteroids in a row.

For each asteroid, the absolute value represents its size, and the sign represents its direction \(positive meaning right, negative meaning left\). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.

Example 1:

```
Input:
asteroids = [5, 10, -5]
Output: [5, 10]
Explanation:
The 10 and -5 collide resulting in 10.  The 5 and 10 never collide.
```

Example 2:

```
Input:
asteroids = [8, -8]
Output: []
Explanation:
The 8 and -8 collide exploding each other.
```

Example 3:

```
Input:
asteroids = [10, 2, -5]
Output: [10]
Explanation:
The 2 and -5 collide resulting in -5.  The 10 and -5 collide resulting in 10.
```

Example 4:

```
Input:
asteroids = [-2, -1, 1, 2]
Output: [-2, -1, 1, 2]
Explanation:
The -2 and -1 are moving left, while the 1 and 2 are moving right.
Asteroids moving the same direction never meet, so no asteroids will meet each other.
```

Note:

* The length of asteroids will be at most 10000.
* Each asteroid will be a non-zero integer in the range \[-1000, 1000\]..

### Solutions

```java
Intuition

According to the question, positive values are moving to the right and negative values are moving to the left. 
We can apply the concept of relative velocity and make positive values as fixed and negative values now moving 
with double velocity in the negative direction. But, magnitude of velocity does not matter only the direction matters.

Idea

Lets consider an example:-

[8, 9, 6, -7, -9, 12, 50, -34]

Start iterating from the beginning. Whenever we encounter a positive value, we don't have to do anything.
 Since they are fixed, they won't attack anyone. But the moment we sees a negative value, we are sure it is 
 going to attack positive values.

Imagine [8, 9, 6] are happily sitting inside the array. The moment -7 enters, it will start knocking out positive 
values. This gives an idea we can use a stack to solve this problem.

Explanation

Lets see how to use this idea to code.

Consider the same example [8, 9, 6, -7, -9, 12, 50, -34]
Initially i = 0.

Whenever we encounter a positive value, we will simply push it to the stack.
The moment we encounter a negative value, we know some or all or zero positive values will be knocked out of the stack. 
The negative value may itself be knocked out or it may enter the stack.
We will keep on poping the elements from the stack while the asteroids[i] > s.top(). But remember, negative asteroids
 can never pop another negative asteroids, since they will never meet them. So, the final condition is while(!s.empty() 
 and s.top() > 0 and s.top() < abs(ast[i])), we will pop the elements.
We have to take care of the case when s.top() == asteroids[i]. In this case one positive element will pop out and 
negative asteroid won't enter the stack.
If after knocking out elements stack becomes empty() or s.top() becomes negative, that means the current asteroids[i] 
will enter the stack.
Code

class Solution {
public:
    vector<int> asteroidCollision(vector<int>& ast) {
        int n = ast.size();
        stack<int> s;
        for(int i = 0; i < n; i++) {
            if(ast[i] > 0 || s.empty()) {
                s.push(ast[i]);
            }
            else {
                while(!s.empty() and s.top() > 0 and s.top() < abs(ast[i])) {
                    s.pop();
                }
                if(!s.empty() and s.top() == abs(ast[i])) {
                    s.pop();
                }
                else {
                    if(s.empty() || s.top() < 0) {
                        s.push(ast[i]);
                    }
                }
            }
        }
        // finally we are returning the elements which remains in the stack.
        // we have to return them in reverse order.
        vector<int> res(s.size());
        for(int i = (int)s.size() - 1; i >= 0; i--) {
            res[i] = s.top();
            s.pop();
        }
        return res;
    }
};
```



