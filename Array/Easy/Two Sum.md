### Quesition:
Given an `array` of integers nums and an `integer` target, return `indices` of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

### Default Thinking:
1. two loops so that every two elements are matched with each other once



### Solution from Me (did not pass):
```Java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int[] result = new int[2];
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int i = 0; i < numbers.length; i++) {
            map.put(numbers[i], i);
            if (map.containsKey(target - numbers[i])) {
                result[0] = i;
                result[1] = map.get(target - numbers[i]);
                return result;
            } 
        }
        return result;
    }
}
```

### Solution from discussion board (passed):
```Java

class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int[] result = new int[2];
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int i = 0; i < numbers.length; i++) {
            if (map.containsKey(target - numbers[i])) {
                result[1] = i;
                result[0] = map.get(target - numbers[i]);
                return result;
            }
            map.put(numbers[i], i);
        }
        return result;
    }
}
```
* Time Complexity: O(N)
* Space Complexity: O(N)

### 反思：
1. 我最大的弱点：不知道如何能实现`不把自身算进去`。在这道题里，最大的难点就是target - nums[i]不能为自身，但可为除自身外与自身value相同的数。
2. 解决方案：把map.put放到后面去。让自身`还未出现`，从而达到`不算自身`的目的
