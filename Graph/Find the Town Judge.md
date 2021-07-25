### Question: 

In a town, there are `n` people labeled from `1` to `n`. There is a rumor that one of these people is secretly the town judge.

If the town judge exists, then:

The town judge trusts nobody.
Everybody (except for the town judge) trusts the town judge.
There is exactly one person that satisfies properties 1 and 2.
You are given an `array` trust where `trust[i] = [ai, bi]` representing that the person labeled `ai` trusts the person labeled `bi`.

Return the label of the town judge if the town judge exists and can be identified, or return `-1` otherwise.

### Default Thinking:

1. Turn it into a graph problem --> use dfs
2. seems that the graph for this question is messy --> 直接从judge must be trusted by everyone except for himself 入手

### My Solution: (Wrong Answer):

```Java
class Solution {
    public int findJudge(int n, int[][] trust) {
        //turn it into a directed graph
        //if there exists one person who is pointed at by everyone except himself
        
        //seems that it cannot be answered by a normal way to solve graphs
        if (trust.length == 1) {
            return trust[0][1];
        }
        
        Map<Integer, Integer> pointed = new HashMap<>();
       
        //put the pointed person as Key (potential judge), put the time it's been trusted as value
        for (int i = 0; i < trust.length; i++) {
            //if the person has not been nominated as Key:
            if (!pointed.containsKey(trust [i][1])) {
                //first time pointed, therefore marked one
                pointed.put(trust[i][1], 1);
                
                System.out.println("* entered " + trust[i][1]);
                //if the person has been nominated as Key:
            } else {
                int value = pointed.get(trust[i][1]);
                value = value + 1;
                pointed.put(trust[i][1], value + 1); 
                
                System.out.println("** entered " + trust[i][1] + "and " +  value);

                if (value == n - 1) {
                    System.out.println("find the judge");
                    return trust[i][1];
                }
            }
        }
        return -1;
        
    }
}
```

### 反思：

1. 我没能cover的点是：当完美的Judge候选人在最后时刻表示他也有trusted的人的话，就违背了principle 2, 因此要return  -1；


### Solution:

思路：
```
1. Create an array of Size N + 1 to represent each person.
   arr[i] represents trust score of i th person
   and arr[i] = number of persons trusts him - number of 
   persons he trusts.
2. Now, traverse through given array. 
    a, b = a trusts b.
    if a person trusts others,
	then decrease his score by 1. i.e, arr[a]--
    if a person is trusted by others, 
    then increase his score by 1. i.e, arr[b]++
3. At last traverse through each person,
    if anyone found with N - 1 trusts,
	then return his index.
4. if not found, return -1
```
重点：！！！！`arr[i] = number of persons trusts him - number of 
   persons he trusts`

```Java
class Solution {
    public int findJudge(int N, int[][] trust) {
        int[] isTrusted = new int[N+1];
        for(int[] person : trust){
            isTrusted[person[0]]--;
            isTrusted[person[1]]++;
        }
        for(int i = 1;i < isTrusted.length;i++){
            if(isTrusted[i] == N-1) return i;
        }
        return -1;
    }
}
```

### 学会的技巧：
1. 反向增加：如果`count`是判断标准，则element作出不符合标准的行为时就将`count` - 1


