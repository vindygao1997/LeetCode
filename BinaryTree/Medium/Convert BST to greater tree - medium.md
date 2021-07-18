### Question:

Given the `root` of a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.

As a reminder, a binary search tree is a tree that satisfies these constraints:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.

### Default thinking:

1. 要traverse --> 用inOrder的话出来的就是ascending
2. 然后存进一个list里
3. 然后每个数字加sum of all numbers after that
4. 最后重新build a tree

### My Solution (did not pass):
```Java
class Solution {
    public TreeNode convertBST(TreeNode root) {
        List<Integer> values = new ArrayList<>();
        // know every key --> traverse to list
        traverse(root, values);
        // calculate the sum of keys greater than original key
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < values.size(); i++) {
            int value = values.get(i);
            int sum = 0;
            // loop through the list and add
            for (int j = 0; j < values.size(); j++) {
                if (j == i) {
                    continue;
                } else {
                    if (values.get(j) > value) {
                        sum += values.get(j);
                    }
                }
            }
            // update keys to the list
            ans.add(sum);
        }
        // build the tree
        
        //dummy node
        TreeNode node = new Node(0);
        // don't know how to contruct a tree
        
    }
    
    public void traverse(TreeNode node, List<Integer> values) {
        if (node == null) {
            return;
        }
        traverse(node.left);
        values.add(node.val);
        traverse(node.right);
    }
}
```

##### 反思：
1. 不知道怎么build a tree是致命点 --> 基础知识不牢固
2. 对BST的性质不熟：原来BST要遵循一个原则：left child < root < right child


### Solution:

```Java
class Solution {
int prev = 0;
  public TreeNode convertBST(TreeNode root) {
    traverse(root);
    return root;
  }
  
  private void traverse(TreeNode cur) {
    if (cur == null) {
      return;
    }
    traverse(cur.right);
    
    cur.val += prev;
    prev = cur.val;
    
    traverse(cur.left);
  }
}
```
解析：

![image](https://user-images.githubusercontent.com/75393672/126079261-fcd0dfc8-f6e6-4749-af28-1213788ae35c.png)
1. 发现了规律就是：由于bst的左小右大规律，导致`all numbers greater than node` 会在`node`的右边
2. 而因为对于每个`node`来说都要求`sum = sum of all numbers greater than node`，所以导致其实答案呼之欲出：每个`node`要增加的数，就是前一个数被转化后的数字
3. 因此有了整段码的精华部分：`cur.val = cur.val + prev;`求出sum, 并更新node的数字。 `prev = cur.val`这就是下一个node需要加的`sum`
4. 由于第一次的Prev = 0,所以需要有一个global variable `prev = 0`

### 反思：

这题的solution是有点讨巧的。

