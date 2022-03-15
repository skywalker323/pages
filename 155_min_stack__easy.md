# 155  Min Stack – Easy

### Problem:

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

push\(x\) — Push element x onto stack.  
pop\(\) — Removes the element on top of the stack.  
top\(\) — Get the top element.  
getMin\(\) — Retrieve the minimum element in the stack.

### Thoughts:

The trick is to use an extra stack to keep the current min value.

### Solutions:

```java

class MinStack {
public:
    //maintaining pair {value,minimum_value}
    stack<pair<int, int>>s;
    MinStack() {}

    void push(int val) {
        if (!s.empty())
            //decide the minimum and then push the pair
            s.push({ val, min(val, s.top().second) });
        else
            //if stack is empty then of course that value itself will be the minimum
            s.push({ val, val });
    }

    void pop() {
        s.pop();
    }

    int top() {
        return s.top().first;
    }

    int getMin() {
        return s.top().second;
    }
};
```



