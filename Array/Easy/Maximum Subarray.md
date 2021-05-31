### Question:
Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the `largest` sum and return its sum.

### Default Thinking:
1. Cannot resolve for the problem that we don't know where to start and don't know where to end.

### Solution (from discussion board):
```Java
class Solution {
    public int maxSubArray(int[] nums) {
        int maxSum = 0;
        int result = nums[0];
        for (int num : nums) {
            maxSum = maxSum > 0 ? maxSum + num : num;
            result = Math.max(result, maxSum);          
        }
        return result;
    }
}
```
* Time Complexity: O(N)
* Space Complexity: O(1)

解释一下上面的逻辑：
```
“To calculate sum(0,i), you have 2 choices: either adding sum(0,i-1) to a[i], or not. 
If sum(0,i-1) is negative, adding it to a[i] will only make a smaller sum, so we add only if it's non-negative.“

Therefore, we have maxSum > 0 ? maSum + num : num;
int result is there to keep track of maxSum, and to make sure that if all values are negative, we have a result = 0;

对我来说，最难理解的部分在于如何确定应该从什么地方开始。
我对上面的理解如下：
1. maxSum 可以等于前面的Positive sum + nums[i], or nums[i].这里是给了个摆脱sum < 0部分的subarray的机会
2. result对比maxSum和自身是给了摆脱negative nums[i]的机会.
3. 因为sum = sum until [i - 1] + nums[i]，所以这样的逻辑一定能找出最大的array sum
```

### 反思
这道题的结题思路是数学：先将sum拆分成sum = sum until [i - 1] + nums[i], 然后再将这两个部分求出最大。
