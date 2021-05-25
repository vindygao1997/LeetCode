用这个文档记录我的default thinking，然后解释为什么是错的，以及如何redirect到good thinking habits

![alt text][logo]

[logo]: https://github.com/vindygao1997/LeetCode/blob/main/Pictures/Screen%20Shot%202021-05-23%20at%2012.41.39%20PM.png

### Default Thinking

for loop -> for each element in `sequence`, loop through `array`.<br>
  If found a match, then break out of the `array` loop
  If reached to the last element in `array` and cannot find a match --> at least one element in `sequence` is not contained in `array`<br>
  Therefore, return `false`.
  
  ```Java
  class Program {
  public static boolean isValidSubsequence(List<Integer> array, List<Integer> sequence) {
		// edge case: empty array;
		if (sequence == null) {
			return true;
		}
		for (int i = 0; i < sequence.size(); i++) {
			for (int j = 0; j < array.size(); j++) {
				if (sequence.get(i) == array.get(j)) {
					break;
				}
				if (j == array.size() - 1) {
					if (sequence.get(i) == array.get(j)) {
						break;
					} else {
						return false;
					}
				}
			}
		}
		return true;
	}
}
```

### Problem:
1. array = {1, 2, 3, 4}, sequence = {1, 1, 2, 3, 4} --> same element with multiple existence
2. 没有正确理解subsequence的含义，sequence的意思是顺序也不能改变，因此array = {1, 2, 3}, sequence = {2, 1, 3} 是return false才对
3. 注意，array = { 1, 3, 2, 4, 5}, sequence = {1, 3, 4, 5} should return `true`


### Solution: Pointer in `array` should only move forward, or move to the right<br>
- IT SHOULD NOT GO BACK TO THE BEGINNING OF THE ARRAY

  - **解决顺序问题（question2 above)**
- At the same time, once one element in `sequence` is found, pointer in `sequence` +1.

  - **解决重复数字的问题（question1）**

### Solution:

```Java
public static boolean isValidSubsequence(List<Integer> array, List<Integer> sequence) {
		// edge case: empty array;
		if (sequence == null) {
			return true;
		}
		int arrayInx = 0;
		int seqInx = 0;
		while (seqInx < sequence.size() && arrayInx < array.size()) {
			if (array.get(arrayInx) == sequence.get(seqInx)) {
				seqInx++;
			}
			arrayInx++;
		}
		return seqInx == sequence.size();
	}
  ```
  
### 反思：
1. 对概念不够了解，`sequence`自带顺序定义，应该要注意
2. 对for loop, while loop的码运行顺序不太熟悉，不知道在loop里面如果有condition, 即if (true)的话，即使满足了true条件，if (true) statement之外的地方还是会被运行
```Java
public static void forLoop(int i) {
  for (int i = 0; i < 4; i++) {
    if (i == 2) {
      System.out.println("Yes I'm here“）；
    }
    System.out.println("I also run here");
  }
}

@Test
forLoop(2); // result: "Yes I'm here I also run here"
```
3. 最后一行`return seqInx == sequence.size()`着实给我纠结了好久，因为default thinking认为需要一个if statement，但是这样又会导致`return statement needed`


### Time and Space Complexity
1. Time = (O)N
2. Space = (O)1




