# 253 LeetCode Java: Meeting Rooms – Medium

### Problem:

Given an array of meeting time intervals consisting of start and end times [[s1,e1],[s2,e2],…] (si < ei), find the minimum number of conference rooms required.

For example,
Given [[0, 30],[5, 10],[15, 20]],
return 2.

### Thoughts:

We still need to sort the intervals by start time in order to make things easier.
A very straightforward way is to have a List of Interval and stored as the occupied interval of a room. And the size of the List will be the number of rooms required.

Because the start time is in increasing order, so that when you found a meeting that needs to be put into one room, it doesn't matter which available room to put in. E.g. if there are three rooms can support a meeting at 1pm and the end time of three rooms are 9 am , 10 am and 11 am. We can arrange this meeting at 1pm to any of the three. Because we know the next (if has next) meeting will start later than 1pm. No matter where we put 1pm meeting, we will have only two rooms available for the later ones.

Because we only care about the end time of a room, so actually we can use a min-heap to keep track of end time so that we don't need to iterate over the list of rooms. Because for a heap. add takes O(logn) in worse case and peek takes O(1) which would be mush faster.

Note that in Java, PriorityQueue is default to be min-heap. If we want to use max-heap, we have to use PriorityQueue pq = new PriorityQueue(11, Collections.reverseOrder());.

### Solutions:

```java
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        vector<pair<int, int>> mts;
        for(int i=0; i < intervals.size(); i++)
        {
          mts.push_back({intervals[i][0], intervals[i][1]});
        }

        sort(mts.begin(), mts.end());
        auto cmp = [](const pair<int, int>& l, const pair<int, int>& r){
            return l.second < r.second;
        };
        typedef pair<int, int> PP;
        multiset<PP, decltype(cmp)> pq(cmp);

        for(auto& m : mts )
        {
            if( pq.empty())
            {
                pq.insert(m);
                continue;
            }

            auto t = *pq.begin();

            if( t.second > m.first )
                pq.insert(m);
            else
            {
                pq.erase(pq.begin());
                pq.insert(m);
            }

        }

        return pq.size();
    }
};
```
