### Question: 

Given the `root` of a binary search tree, rearrange the tree in in-order so that the leftmost node in the tree is now the root of the tree, and every node has no left child and only one right child.

### Default Thinking:

只要学会了binary tree traversal就可以做这道题

实际发现我不会的是：
1. 不知道要用list来建立tree 
2. 不知道怎么样把traversal的结果一个一个地加入到想加的地方

### Solution: 

```Java
class Solution {    
    public TreeNode increasingBST(TreeNode root) {
        List<Integer> vals = new ArrayList();
        inorder(root, vals);
        TreeNode ans = new TreeNode(0), cur = ans;
        for (int v: vals) {
            cur.right = new TreeNode(v);
            cur = cur.right;
        }
        return ans.right;
    }

    public void inorder(TreeNode node, List<Integer> vals) {
        if (node == null) return;
        inorder(node.left, vals);
        vals.add(node.val);
        inorder(node.right, vals);
    }
}
```

### 分析：

1. 首先第一个技巧，把traversal的东西放进list里的方法是在traverse的时候就把list作为其中一个input,然后instead of print, 直接用list.add()将每个element加进去
2. 第二个技巧 （我并没有懂的地方）：建立一个answer的reference（cur) 来放答案，而不是直接放到ans里
3. 第三个技巧：直接return `ans.right`。这个我也没懂，为什么就能把一连串都return出来
