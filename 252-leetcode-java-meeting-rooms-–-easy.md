# 252 Meeting Rooms II – Easy

### Problem:

Given an array of meeting time intervals consisting of start and end times [[s1,e1],[s2,e2],…] (si < ei), determine if a person could attend all meetings.

For example,
Given [[0, 30],[5, 10],[15, 20]],
return false.

### Thoughts:

We can solve this problem easily by sorting the Intervals. But there is no custom comparator for it so that we need to create one.

We want to sort the Interval by the start time, then if a person can attend all meetings, the start time of a meeting must be after the end time of the previous one.

### Solutions:

```java
class Solution {
public:
    bool canAttendMeetings(vector<vector<int>>& intervals) {
        vector<pair<int, int>> mts;
        for(auto i : intervals)
        {
            mts.push_back({i[0], i[1]});
        }

        sort(mts.begin(), mts.end(), std::less<pair<int, int>>());
        for(int i= 1; i < mts.size(); i++)
        {
            if( mts[i-1].second > mts[i].first)
                return false;
        }
        return true;
    }
};
```
