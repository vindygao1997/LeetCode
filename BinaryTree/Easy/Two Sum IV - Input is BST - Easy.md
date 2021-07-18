### Question

Given the `root` of a Binary Search Tree and a target number `k`, return `true` if there exist two elements in the BST such that their sum is equal to the given target.

### Default Thinking

将two sum的做法结合在helper method那里

### My solution:
```Java
class Solution {
    public boolean findTarget(TreeNode root, int k) {
        //find target: map
        Map<Integer, Integer> values = new HashMap<>();
        //extract keys and pair it with its complementary part
        helper(root, values, k);
        if (values.containsKey(100000)) {
            return true;
        }
        return false;
        
    }
    
    public void helper(TreeNode node, Map<Integer, Integer> values, int k) {
        if (node == null) {
            return;
        }
        helper(node.left, values, k);
        
        if (values.containsKey(k - node.val)) {
            values.put(100000, null);
        } else {
            values.put(node.val, null);
        }
        
        helper(node.right, values, k);
    }
}
```
##### 反思：
1. containsKey(100000)是作弊
2. 一开始想不起来用map



### 升级版本的my solution:

1. 先inOrder traverse得到sorted list
2. 再用two pointer来找sum

```Java
class Solution {
    public boolean findTarget(TreeNode root, int k) {
        List<Integer> values = new ArrayList<>();
        //inOrder traverse create a sorted list;
        helper(root, values);
        //use two pointers
        int front = 0;
        int back = values.size() - 1;
        while (front < back) {
            int sum = sum(values, front, back);
            if (sum < k) {
                front++;
            } 
            if (sum > k) {
                back--;
            } 
            if (sum == k) {
                return true;
            }
        }
        return false;
    }
    
    public void helper(TreeNode node, List<Integer> values) {
        if (node == null) {
            return;
        }
        helper(node.left, values);
        values.add(node.val);
        helper(node.right, values);
    }
    
    public int sum(List<Integer> values, int frontIndex, int lastIndex) {
        return values.get(frontIndex) + values.get(lastIndex);
    }
}
```
##### 改进之处：
1. 不用作弊，用了two sum比较灵巧
2. 用list也比map用的空间少
3. 耗时减少

### Solution1:

1. 用stack: 使得栈顶元素是整个BST最小， stack可以控制只出栈顶元素 pop()， 所以不会漏掉元素
2. set用的比map更灵活

```Java

class Solution {
    public boolean findTarget(TreeNode root, int k) {
        //edge case
        if (root == null) {
            return false;
        }
        Deque<TreeNode> stack = new ArrayDeque<>();
        Set<Integer> set = new HashSet<>();
        
        pushLeft(stack, root);
        
        while (! stack.isEmpty()) {
            TreeNode cur = stack.pop();
            if (set.contains(k - cur.val) {
                return true;
            }
            set.add(cur.val);
            
            // IMPORTANT:: need to traverse the right children as well!!!
            pushLeft(cur.right);
        }
        return false;
    }
    
    private void pushLeft(Deque<TreeNode> stack, TreeNode node) {
        while (node != null) {
            stack.push(node);
            // all the way down the left childrens so that we reach the smallest value
            node = node.left
        }
    }
}
```


### Solution2:

The simplest solution will be to traverse over the whole tree and consider every possible pair of nodes to determine if they can form the required sum kk. But, we can improve the process if we look at a little catch here.

If the sum of two elements `x + y` equals `k`, and we already know that `x` exists in the given tree, we only need to check if an element `y` exists in the given tree, such that `y = k - x`. Based on this simple catch, we can traverse the tree in both the directions(left child and right child) at every step. We keep a track of the elements which have been found so far during the tree traversal, by putting them into a `set`.

For every current node with a value of `p`, we check if `k−p` already exists in the array. If so, we can conclude that the sum `k` can be formed by using the two elements from the given tree. Otherwise, we put this value `p` into the `set`.

If even after the whole tree's traversal, no such element `p` can be found, the sum `k` can't be formed by using any two elements.


小技巧：

1. 对于每一个已经traverse到的`node.val`，我们有目的地traverse去寻找`k - node.val`
2. 直接对比，无需将traverse出来的元素再存进另一个地方
3. 同时traverse left child 和 right child：用||


```Java
public class Solution {
    public boolean findTarget(TreeNode root, int k) {
        Set < Integer > set = new HashSet();
        return find(root, k, set);
    }
    public boolean find(TreeNode root, int k, Set < Integer > set) {
        if (root == null)
            return false;
        if (set.contains(k - root.val))
            return true;
        set.add(root.val);
        return find(root.left, k, set) || find(root.right, k, set);
    }
}
```









