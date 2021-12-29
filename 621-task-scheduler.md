# 621 Task Scheduler

### Problem

Given a char array representing tasks CPU need to do. It contains capital letters A to Z where different letters represent different tasks.Tasks could be done without original order. Each task could be done in one interval. For each interval, CPU could finish one task or just be idle.

However, there is a non-negative cooling interval n that means between two same tasks, there must be at least n intervals that CPU are doing different tasks or just be idle.

You need to return the least number of intervals the CPU will take to finish all the given tasks.

Example 1:  
Input: tasks = \["A","A","A","B","B","B"\], n = 2  
Output: 8  
Explanation: A -&gt; B -&gt; idle -&gt; A -&gt; B -&gt; idle -&gt; A -&gt; B.  
Note:  
The number of tasks is in the range \[1, 10000\].  
The integer n is in the range \[0, 100\].

### Solutions

```java
The idea is to find a way to arrange these tasks, and use as less idle intervals as possible.

By observing a few test cases, we know that it's a lot easier to start from the task with the highest frequency of
 occurrence. Let's say the task is A.
Then we can put all A tasks into the array, and make sure they are separated by n idle inervals.
A____A____A____A____A____A

number of A = num(A)            // the number of task A
gap = (num(A) - 1) x n          // the number of idle intervals
total length = num(A) + gap     // the total length
Now we need to replace the idle intervals with other tasks. Similarily, we'd better start from tasks with the second 
highest frequency of occurrence. Let's say it's B, then
a) if num(B) < num(A) && num(B) < gap, we know that the total length will not be affected. We simply decrement the
 gap by num(B).
AB___AB___AB____A____A____A
b) if num(B) == num(A) && num(B) < gap, we need to decrement the gap by num(B) - 1, and also increment total length
 by 1.
AB___AB___AB___AB___AB___AB
c) If we run out of gap, it is great, because this case is even easier to handle. It means we don't need any idle 
intervals at all. Simply return the length of all tasks, and we are done. Why? Let's say We have all gaps filled,
 we have an array like this:
ABCDABCDABCDABCDABCD
Now we need to insert task E, we can change the filled gaps a little bit, make extra spaces for task E, the number 
of changed gaps depends on how many E we have, then the array looks like:
ABCD_ABCD_ABCDABCDABCD
With E inserted:
ABCDEABCDEABCDABCDABCD
If E repeats as many times as A, the array will look like:
ABCDEABCDEABCDEABCDEABCDE
We can always find a way to arrange them without introducing idle intervals.
So we have this piece of code.

    int leastInterval(vector<char>& tasks, int n) {
        int m[128] = {0};
        for (char c : tasks)  m[c]++;  
        sort(m, m + 128, [](int a, int b){ return a > b; });
        int gap = n * (m[0] - 1), total = m[0] + gap;
        for (int i=1; i<128 && m[i] != 0; i++) {
            if (gap >= m[i]) {
                if (m[i] == m[0]) {
                    gap -= (m[0] - 1);      
                    total++;
                } else {
                    gap -= m[i];
                }
            } else {
                return tasks.size();
            }
        } 
        return total;
    }
```



