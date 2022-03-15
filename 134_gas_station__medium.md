# 134 Gas Station – Medium

### Problem:

There are N gas stations along a circular route, where the amount of gas at station i is gas\[i\].

You have a car with an unlimited gas tank and it costs cost\[i\] of gas to travel from station i to its next station \(i+1\). You begin the journey with an empty tank at one of the gas stations.

Return the starting gas station’s index if you can travel around the circuit once, otherwise return -1.

Note:  
The solution is guaranteed to be unique.

### Thoughts:

Here is the key thing to keep in mind.

1 If total cost is greater than total amount of gas, there is no way to make a circle travel

2 We have a current remaining which it the remaining amount of gas to start from one of the gas stations. Because there is only one unique solution. For all  other gas station, there must be a point that the current remaining is negative which means it cannot start from currently marked station.

### Solutions:

```cpp
class Solution {
  public:
  int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
    int n = gas.size();

    int total_tank = 0;
    int curr_tank = 0;
    int starting_station = 0;
    for (int i = 0; i < n; ++i) {
      total_tank += gas[i] - cost[i];
      curr_tank += gas[i] - cost[i];
      // If one couldn't get here,
      if (curr_tank < 0) {
        // Pick up the next station as the starting one.
        starting_station = i + 1;
        // Start with an empty tank.
        curr_tank = 0;
      }
    }
    return total_tank >= 0 ? starting_station : -1;
  }
};
```



