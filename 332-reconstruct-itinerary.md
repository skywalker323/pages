# 332. Reconstruct Itinerary

### Problem:

<pre>
Given a list of airline tickets represented by pairs of departure and arrival airports [from, to], reconstruct the itinerary in order. All of the tickets belong to a man who departs from JFK. Thus, the itinerary must begin with JFK.

Note:
If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string. For example, the itinerary ["JFK", "LGA"] has a smaller lexical order than ["JFK", "LGB"].
All airports are represented by three capital letters (IATA code).
You may assume all tickets form at least one valid itinerary.
Example 1:
tickets = [["MUC", "LHR"], ["JFK", "MUC"], ["SFO", "SJC"], ["LHR", "SFO"]]
Return ["JFK", "MUC", "LHR", "SFO", "SJC"].
Example 2:
tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
Return ["JFK","ATL","JFK","SFO","ATL","SFO"].
Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"]. But it is larger in lexical order.
</pre>

### Solutions:

```java
public class Solution {
    public List<String> findItinerary(String[][] tickets) {
        HashMap<String, List<String>> flights = new HashMap<String, List<String>>();
        List<String> remain = new LinkedList<String>();
        for (int i = 0; i < tickets.length; i ++) {
            if (!flights.containsKey(tickets[i][0])) {
                flights.put(tickets[i][0], new LinkedList<String>());
            }
            flights.get(tickets[i][0]).add(tickets[i][1]);
            remain.add(tickets[i][0] + tickets[i][1]);
        }
        for (String c:flights.keySet()) {
            Collections.sort(flights.get(c));
        }
        return fi(tickets.length + 1, "JFK", flights, new LinkedList<String>(), remain);
    }
    private List<String> fi(int n, String port, HashMap<String, List<String>> flights, List<String> curr, List<String> remain) {
        curr.add(port);
        if (curr.size() == n) {
            return new LinkedList<String>(curr);
        }
        if (flights.containsKey(port)) {
            for (String next:flights.get(port)) {
                if (!remain.contains(port + next)) {
                    continue;
                }
                remain.remove(port + next);
                List<String> result = fi(n, next, flights, curr, remain);
                if (result != null) {
                    return result;
                }
                remain.add(port + next);
            }   
        }
        curr.remove(curr.size() - 1);
        return null;
    }
}
```

```java
// iterative approach
class Solution {
public:
	vector<string> findItinerary(vector<vector<string>> tickets) {
		// Each node (airport) contains a set of outgoing edges (destination).
		unordered_map<string, multiset<string>> graph;
		// We are always appending the deepest node to the itinerary, 
		// so will need to reverse the itinerary in the end.
		vector<string> itinerary;
		if (tickets.size() == 0){
			return itinerary;
		}
        
		// Construct the node and assign outgoing edges
		for(vector<string>& eachTicket : tickets){
			graph[eachTicket[0]].insert(eachTicket[1]);
		}
		stack<string> dfs;
		dfs.push("JFK");
		while (!dfs.empty()){
			string topAirport = dfs.top();
			if (graph[topAirport].empty()){
				// If there is no more outgoing edges, append to itinerary
				// Two cases: 
				// 1. If it searchs the terminal end first, it will simply get
				//    added to the itinerary first as it should, and the proper route
				//    will still be traversed since its entry is still on the stack.
				// 2. If it search the proper route first, the dead end route will also
				//    get added to the itinerary first.
				itinerary.push_back(topAirport);
				dfs.pop();
			}
			else {
				// Otherwise push the outgoing edge to the dfs stack and 
				// remove it from the node.
				dfs.push(*(graph[topAirport].begin()));
				graph[topAirport].erase(graph[topAirport].begin());
			}
		}
		// Reverse the itinerary.
		reverse(itinerary.begin(), itinerary.end());
		return itinerary;
	}
};
```