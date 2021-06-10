### Question:

Given an integer array `nums` of unique elements, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

### Default Thinking:

我没考虑到[1, 2, 3] 里面会需要有[1,2]和[1,3]；我只得出了[],[1],[1,2],[1,2,3].

### Solution:
```Java
     public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> temp = new ArrayList<Integer>();
        // add an empty set
        result.add(temp);
        //思路是：每次加新element,
        for (int num : nums) {
            int n = result.size();
            for (int i = 0; i < n; i++) {
                temp = new ArrayList<Integer>(result.get(i));
                temp.add(num);
                result.add(temp);
            }
        }
        return result;
    }
```

##### 解释：
```
最重要的点就是发现，每次增加一个新element，其实是相当于将新element加在已有的list上，然后再将曾有的和新的combine
for example：[1, 2, 3]
round 1: []
round 2: add 1 to [], getting [1], then combine the two : [[],[1]]
round 3: add 2 to [] and [1], getting [2],[1,2], then combine: [[],[1],[2],[1,2]]
round 4: add 3 to [],[1],[2],[1,2], getting [3],[1,3],[2,3],[1,2,3], then combine: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

### 拓展：how about permutations?

### 总结反思：对这类型的题目找不到暴力解法
