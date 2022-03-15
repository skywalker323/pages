# 133 Clone Graph – Medium

### Problem:

Clone an undirected graph. Each node in the graph contains a label and a list of its neighbors.

OJ’s undirected graph serialization:Nodes are labeled uniquely.We use \# as a separator for each node, and , as a separator for node label and each neighbor of the node.As an example, consider the serialized graph {0,1,2\#1,2\#2,2}.  
The graph has a total of three nodes, and therefore contains three parts as separated by \#.

First node is labeled as 0. Connect node 0 to both nodes 1 and 2.  
Second node is labeled as 1. Connect node 1 to node 2.  
Third node is labeled as 2. Connect node 2 to node 2 \(itself\), thus forming a self-cycle.  
Visually, the graph looks like the following:

```
   1
  / \
 /   \
0 --- 2
     / \
     \_/
```

### Thoughts:

This problem actually have an assumption that’s not given out. This graph has to be a connected graph. If it’s not connected, it’s impossible to clone a graph give only one node.

We need to copy each single node and assign the correct reference to the copied node.

Easily made mistake is to assign pointer to the old reference.

The solution below relies on each node has a unique value because it uses label to uniquely find the copied node. It’s also using a BFS style to traverse the graph.

### Solutions:

```java
class SolutionNode {
public:
    Node* cloneGraph(Node* node) {
        if (!node)  return nullptr;
        vector<Node*> visited(101);
        queue<Node*> q;
        q.push(node);
        const auto new_node = new Node(node->val);
        visited[new_node->val] = new_node;
        while(!q.empty()){
            const int size = q.size();
            for(int i=0; i<size; i++){
                const auto old_node = q.front();
                q.pop();
                for(auto val : old_node->neighbors){
                    if (!visited[val->val]){
                        auto adj_node = new Node(val->val);
                        visited[adj_node->val] = adj_node;
                        visited[old_node->val]->neighbors.push_back(adj_node);
                        q.push(val);
                    } else {
                        visited[old_node->val]->neighbors.push_back(visited[val->val]);
                    }
                }
            }
        }
        return visited[node->val];
    }
};
```



