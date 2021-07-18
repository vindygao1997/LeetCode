### Question:

Convert a BST to a sorted circular doubly-linked list in-place. Think of the left and right pointers as synonymous to the previous and next pointers in a doubly-linked list.

![image](https://user-images.githubusercontent.com/75393672/126048731-20215d22-9895-4f13-b5de-7ff284ad27b5.png)
![image](https://user-images.githubusercontent.com/75393672/126048742-ffb6e0d8-a5e8-497c-b744-aa6146802695.png)
![image](https://user-images.githubusercontent.com/75393672/126048746-8d58c754-8d45-4f86-9081-b8f677e64ad2.png)### Default thinking:


### My solution:

```Java
public class DLL {
    Node head; // head of list
 
    /* Doubly Linked list Node*/
    class Node {
        int data;
        Node prev;
        Node next;
 
        // Constructor to create a new node
        // next and prev is by default initialized as null
        Node(int d) { data = d; }
    }
    
    class TreeNode{
        int val;
        TreeNode left;
        TreeNode right;
        
        TreeNode(int value) {
            val = value;
        }
    }


     public Node treetoDoublyList (TreeNode root){
        if (root == null) {
            return root;
        }
        DLL llist = new DLL();
        inOrder(root, llist);
        Node ans = new Node(0);
        Node cur = ans;
        //不会写了
        
     }
     
     public void inOrder(TreeNode node, DLL llist) {
         if (node == null) {
             return;
         }
         inOrder(node.left, llist);
         append(node.val, llist);
         inOrder(node.right, llist);
     }
     
     public void append(int newValue, DLL llist) {
         Node new_node = new Node(newValue);
         new_node.next = null;
         if (llist.head == null) {
             new_node.prev = null;
             llist.head = new_node;
             return;
         }
         Node last = head;
         while (last != null) {
             last = last.next;
         }
         last.next = new_node;
         new_node.prev = last;
     }
}
```

##### 反思：
1. 应该怎样去处理linkedlist, treenode, node的nested关系
2. 为什么最后得出的return value会是`Node`

### My Solution改进版:

```Java
public class Solution{
    Node prev = null;
    Node head = null;

     public Node treeToDoubleList(Node root){
         if (root == null) {
             return root;
         }
         inOrder(root);
        //if it is the last one --> connect to the first one
        prev.right = head;
        return head;
     }
     
     public void inOrder(Node node) {
         if (node == null) {
             return;
         }
         inOrder(node.left);
         
         //if it is the first one --> become the "head"
         if (head == null) {
             head = node.val;
             prev = head;
         } else {
             //if it is not the first one --> "prev" == left, "next" == right
             prev.right = node;
             node.left = prev;
             prev = node;
         }
         
         inOrder(node.right);
     }
     
}
```

### Solution:

```Java

class Solution {

    Node first = null;
    Node last = null;
    
    public Node treeToDoublyList(Node root) {
        if (root == null) {return root;}
        
        helper(root);
        
        last.right = first;
        first.left = last;
        
        return first;
    }
    
    public void helper(Node node) {
        if (node != null) {
        
            helper(node.left);
            
            if (last != null) {
                last.right = node;
                node.left = last;
            } else {
                first = node;
            }
            last = node;
            
            helper(node.right);
        }
    }
         
}
```
        


### 反思与改进：

概念不了解：

1. 对linkedlist的概念并不了解 --> 只要是一个node,有一个或多个pointer指向另一个node,即为linkedlist。它不一定是从list那边来。
2. 对treenode和linkedlist的node之间的联系不了解 --> 他们其实是一样的。treenode的`node.left`和`node.right`相当于linkedlist里面的`node.prev`和`node.next`

技巧不了解：

1. 处理linkedlist的时候，不取`node.val`，而是直接用`prev = node`这种形式 --> 原因：本质上是两个node之间的pointer关联，而不是node的value
2. 建立doubly linkedlist的关键：在每一步都建立`双向pointer`，即`prev.right = node`和`node.left = prev`
3. 向linkedlist的下一个node移动的方法：将`现在`变为prev,即`prev = node`,或者将`现在`变为next,即`node = node.next`

return type不了解：

1. 本质上是对node的表现方式不了解。因为觉得需要return一条链子，而认为要return list/array/tree --> 其实是return node. 不管是treenode还是linkedlist node,node存在的意义就是连成一串。

