### Quesiton:
Given an array `nums` of `n` integers where `nums[i]` is in the range `[1, n]`, return an array of all the integers in the range [1, n] that `do not` appear in `nums`

### Default Thinking:
1. 关键是产生那些不存在的数字，然后存起来
2. 尽量不要为了产生一个数字而loop一次原array

### Solution 
1. HashMap (inspired by lty)
```Java
import java.util.Map.Entry;
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        int n = nums.length;
        List<Integer> myList = new ArrayList<>();
        
        Map<Integer, Integer> myMap = new HashMap<>();
        //put every value in `range` to a map with count 0;
        for (int i = 1; i < n + 1; i++) {
            myMap.put(i, 0);
        }
        //once each value appears in `array`, increase count (and this will avoid adding the same value twice in the list)
        for (int num : nums) {
            if (myMap.containsKey(num)) {
                myMap.put(num, myMap.get(num) + 1);
            }
        }
        //iterate through map to find key with count == 0, which means those values have never appeared in `array`
        for (Entry<Integer, Integer> entry : myMap.entrySet()) {
            if (Objects.equals(0, entry.getValue())) {
                myList.add(entry.getKey());
            }
        }
        return myList;
    }
}
```
* Time Complexity: O(3N) = O(N)
* Space Complexity: O(N), because map with increase when n increases


2. Swaping (In-place solution)
```Java
    public List<Integer> findDisappearedNumbers(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
        //swap the value of nums[i] to the right position if ascending (which is the same as `range`)
            while (nums[i] != nums[nums[i] - 1]) {
                int tmp = nums[i];
                nums[i] = nums[tmp - 1];
                nums[tmp - 1] = tmp;
            }
        }
        List<Integer> res = new ArrayList<Integer>();
        // find missing values
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != i + 1) {
                res.add(i + 1);
            }
        }
        return res;
    }
}
```
* Time Complexity: O(N)
* Space Complexity: O(1)

解释一下这个swaping的逻辑：
```
Ideally nums[] holds values in ascending order. In other words, nums[i] = i + 1.
e.g.
nums[]: 1, 2, 3, 4, 5, ..., n-1, n
index(i): 0, 1, 2, 3, 4, ..., n-2, n-1

nums[i] != i + 1 checks whether nums[i] is in the right position(index).
e.g.
nums[]: 4, 3, 2, 7, 8, 2, 3, 1
When i = 0, we check position 0. The position 0 should hold number 1 (i + 1), while it holds number 4.
This means number 4 (nums[i]) is not in the right position.

nums[i] != nums[nums[i] - 1] checks whehter the right position for nums[i] is occupied with another number.
e.g.
nums[]: 4, 3, 2, 7, 8, 2, 3, 1
When i = 0, the right postion for number 4 is 3 (nums[i] - 1). While postion 3 now is occupied by number 7.
This means the right postion (3) holds incorrect value.

When the two conditions are met, it means the current number is not in the right postion and the right postion has wrong value.
So we swap nums[i] and nums[nums[i] - 1] to make sure at least position (nums[i] - 1) holds the right number.

BTW, we actually just need to check the second condition: nums[i] != nums[nums[i] - 1]
```

3. Use values as Index
```Java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> ans = new ArrayList<Integer>();
		int arr[] = new int[nums.length + 1]; // Take an array to visit all the elements present in the 'nums' array
		for (int i = 0; i < nums.length; i++)
			arr[nums[i]]++; // Visti all the elements in 'nums' array
		for (int i = 1; i < arr.length; i++) { // The Element starts from 1 to n, so ignore 0.
			if (arr[i] == 0) // If you find an element unvisited. That's the missing number.
				ans.add(i);
		}
		return ans;
    }
}
```
* Time Complexity: O(N)
* Space Complexity: O(N), extra space since a new array is needed
