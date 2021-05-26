### Major Elements - Easy (LC #169)

Given an `array` nums of size `n`, return the `majority` element.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

### Default Thinking:
1. traverse through the whole `array`, count每一个array[i]的重复，当Count == array.length / 2 时， return `array[i]`

### Problem:
1. Timed out --> traverse array次数太多
2. Space complexity up --> 最坏情况要存下 == array.length那么多的count

### Solution:

1. Map (my solution)
```Java
public int majorityElement(int[] nums) {
        // how to keep track of every element? --> need to update count everytime it exists
        // best for updating --> and pairs --> map
        // if the map does not contain that element as key --> put (element, 1);
        // if the map contains that element as key --> extract value, value + 1, put (element, newValue);
        // if newValue > length / 2 --> return key
        Map<Integer, Integer> map = new HashMap<>();
        int result = 0;
        if (nums.length == 1) {
            result = nums[0];
        }
        for (int num : nums) {
            if (!map.containsKey(num)) {
               map.put(num, 1); 
            } else {
                int value = map.get(num);
                value++;
                if (value > (nums.length / 2)) {
                    result = num;
                } else {
                    map.put(num, value);             
                }
            }
        }
        return result;
    }
  ```
 Time: O(N)
 Space: O(N)
 
 2. Sorting
 ```Java
 public int majorityElement1(int[] nums) {
    Arrays.sort(nums);
    return nums[nums.length/2];
}
```
* Time: O(NlogN)
* Space: since Arrays.sort is a tuned quicksort that involves mergesort, the space complexity is between O(1) and O(N) 
  * Warning: I can be wrong

3. Moore Voting Algorithm

```Java
class Solution {
    public int majorityElement(int[] nums) {
        //assume nums[i] is the majority element
        // if nums[i + 1] == nums[i] --> increment count
        // if !=, decrement count
        // once count hits zero, switch majority element, and change count to 1
        // verify the majority element by counting it through the whole array and compare count to N/2
        
        // find majorElement
        int result = 0;
        int majorElement = 0;
        int count = 0;
        for (int num : nums) {
            if (count == 0) {
                majorElement = nums[i];
                count = 1;
            }
            if (nums[i] == majorElement) {
                count++;
            } else {
                count--;
            }
        }
        // verify that majorElement has count > nums.length / 2
        for (int num : nums) {
            if (num == majorElement) {
                count++;
            }
        }
        if (count > nums.length / 2) {
            result = majorElement;
        }
        return result;
    }
}
```
* sometimes the "verify" part will be omitted
* Time complexity: O(N) -- linear time (much quicker than HashMap solution when run on LeetCode)
* Space complexity: O(1) -- constant space

4. Bit Manipulation
```
public int majorityElement(int[] nums) {
    int[] bit = new int[32];
    for (int num: nums)
        for (int i=0; i<32; i++) 
            if ((num>>(31-i) & 1) == 1)
                bit[i]++;
    int ret=0;
    for (int i=0; i<32; i++) {
        bit[i]=bit[i]>nums.length/2?1:0;
        ret += bit[i]*(1<<(31-i));
    }
    return ret;
}
```


 
