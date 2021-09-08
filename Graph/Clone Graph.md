### Clone Graph
Given a reference of a node in a connected undirected graph.

Return a deep copy (clone) of the graph.

Each node in the graph contains a value (int) and a list (List[Node]) of its neighbors.

```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

Test case format:

For simplicity, each node's value is the same as the node's index (1-indexed). For example, the first node with val == 1, the second node with val == 2, and so on. The graph is represented in the test case using an adjacency list.

An adjacency list is a collection of unordered lists used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with val = 1. You must return the copy of the given node as a reference to the cloned graph.

### My Code:

##### 寻找为什么不对

```Java
class Solution {
    public Node cloneGraph(Node node) {
        if (node == null) return null;
        Map<Integer, Node> map = new HashMap<>();
        return cloneGraph(node, map);
        
    }
    
    private Node cloneGraph(Node node, Map<Integer, Node> map) {
        //access the neighbor nodes : copyGraph(node.neighbors, map) -> used as recursion
        
        //return copy of the given node -> after traversing all nodes, return -> when a repetition appears in map, return
        if (map.containsKey(node.val)) {
            return map.get(node.val);
        }
        //map stores new copy : key == node.val, value == node.neighbors
        
        //deep copy: therefore creating new node objects and copy the values
        Node copyNode = new Node(node.val);
        map.put(node.val, copyNode);
        //copyNode.neighbors = node.neighbors; //doesn't look right
        List<Node> list = new ArrayList<>();
        
        
        //what's wrong with this one??
        //////////////////////////////////////
        //List<Node> list = new ArrayList<>();
        //for (Node neighbor : node.neighbors) {
          //  list.add(copyGraph(neighbor, map));
        //}
        ////////////////////////////////////////////////

        for (Node neighbor : node.neighbors) copyNode.neighbors.add(cloneGraph(neighbor, map));
        
        //after dealing with the current node, we want to traverse to the next one
        return copyNode;
    }
}
```


