### Question: 
Given an integer `array` of size n, find all elements that appear more than `⌊ n/3 ⌋` times.

### Default Thinking:
1. 跟easy的很像，但是需要“find all elements" 而不像easy一样结果只会有一个，所以用Map

### Problem: 
1. Solution (1): HashMap --> passed with low time and space complexity
```Java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        //once an element appears more than [n / 3] times, we store it in a list --> map
        //store them in a list
        List<Integer> myList = new ArrayList<>();
        Map<Integer, Integer> myMap = new HashMap<>();
        // if length <=2, every element has a count > length / 3
        if (nums.length <= 2) {
            if (nums.length == 1) {
                myList.add(nums[0]);
            } else {
                if (nums[0] != nums[1]) {
                    myList.add(nums[0]);
                    myList.add(nums[1]);
                } else {
                    myList.add(nums[0]);
                }
            }
        }
        // if length >= 3
        if (nums.length > 2) {
            for (int num : nums) {
                // add new candidates
                if (!myMap.containsKey(num)) {
                    myMap.put(num, 1);
            } else {
                    // existing candidates, determine if it should be transferred to myList
                    if (myMap.get(num) < nums.length / 3) {
                        myMap.put(num, myMap.get(num) + 1);
                    } else {
                        if (!myList.contains(num)) {
                            myList.add(num);
                        }
                    }
                }
            }
        }
        
        return myList;
    }
}
```
#### 分析：
* Map的time complexity = O(N), Space complexity = O(N) *(I can be wrong)*, 比较慢


2. Moore's Voting Algorithm
```Java
public List<Integer> majorityElement(int[] a) {
  // we can only have maximum 2 majority elements
  int n = a.length;
  int c1 = 0, c2 = 0;
  Integer m1 = null, m2 = null;
  
  // step 1. find out those 2 majority elements
  // using Moore majority voting algorithm
  for (int i = 0; i < n; i++) {
    if (m1 != null && a[i] == m1)      { c1++; } 
    else if (m2 != null && a[i] == m2) { c2++; }
    else if (c1 == 0)                  { m1 = a[i]; c1 = 1; }
    else if (c2 == 0)                  { m2 = a[i]; c2 = 1; } 
    else                               { c1--; c2--; }
  }
  
  // step 2. double check
  c1 = 0; c2 = 0;
  
  for (int i = 0; i < n; i++) {
    if (m1 != null && a[i] == m1) c1++;
    if (m2 != null && a[i] == m2) c2++;
  }
  
  List<Integer> res = new ArrayList<Integer>();
  
  if (c1 > n / 3) res.add(m1);
  if (c2 > n / 3) res.add(m2);
  
  return res;
}

```
* 这里的c1,c2是count for candidate1, candidate2

#### 分析：
* 关键是要判断出最多只能有`两个`数字是`count > n/3`的 
  
