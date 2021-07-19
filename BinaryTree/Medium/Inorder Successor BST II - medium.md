### Question:

Given a  `node` in a binary search tree, return the in-order successor of that node in the BST. If that node has no in-order successor, return `null`

The successor of a `node` is the node with the smallest key greater than the `node.val`

You will have direct access to the node but not the root of the tree. Each node will have a reference to its parent node.



<img width="863" alt="Screen Shot 2021-07-18 at 10 49 52 PM" src="https://user-images.githubusercontent.com/75393672/126100158-597fdd8a-7127-4df8-9a0e-142b219beb2a.png">

<img width="607" alt="Screen Shot 2021-07-18 at 10 53 32 PM" src="https://user-images.githubusercontent.com/75393672/126100494-bc5c7956-de23-4551-afce-5eb3eecd1469.png">

<img width="628" alt="Screen Shot 2021-07-18 at 10 54 59 PM" src="https://user-images.githubusercontent.com/75393672/126100566-4917a45f-5ad7-490a-a506-ca0091777328.png">

<img width="623" alt="Screen Shot 2021-07-18 at 10 55 43 PM" src="https://user-images.githubusercontent.com/75393672/126100641-d27abf4a-1592-4be3-8404-3e3a420d1443.png">

### Concepts:

in-order successor: 

if `node.val` == 11, in-order successor is the smallest `val` that's greater than 11. Such as `12`

From examples above, we can see that the successor can occur in :

1. node.right
2. one of the left child of node.right
3. the parent of node, if the node was a left child 
4. the parent's parent of the node, or the parent's parent's right child, if the node was a right child

### Key:

如果这道题不给`node.parent`，则只能用个map或者什么别的东西去寻`node`对应的parent,会比较麻烦。

具体详细见: [863. All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/solution/)

### Solution:

```Java

class Solution {
  public Node inorderSuccessor(Node node) {
    if (node == null) {
      return null;
    }
    
    Node descendent = findTheDescendent(node.right);
    
    if (descendent == null) {
      return findTheAncestor(node);
    }
    return descendent;
  }
  
  private Node findTheDescendent(Node node) {
    if (node == null) {
      return null;
    }
    while (node.left != null) {
      node = node.left;
    }
    return node;
  }
  
  private Node findTheAncestor(Node node) {
    while (node.parent != null) {
      if (node.val < node.parent.left) {
        return node.parent.left;
      }
      node = node.parent;
    }
    return null;
  }
}
```


