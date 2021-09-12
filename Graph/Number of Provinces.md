### 547. Numebr of Provinces

There are n cities. Some of them are connected, while some are not. If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c.

A province is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an n x n matrix isConnected where isConnected[i][j] = 1 if the ith city and the jth city are directly connected, and isConnected[i][j] = 0 otherwise.

Return the total number of provinces.

![image](https://user-images.githubusercontent.com/75393672/132963677-a27dd48c-b880-4afd-9306-bda5d01b3207.png)


### 问题：

1. 既然edges已经用1表示了，应该怎么利用这点来找neighbors呢？ --> 遇到1，说明有neighbors,可以开始dfs穷尽所有neighbors,得到一个province
2. 即使是0也不代表两个人不在一个province，所以应该用什么情况让dfs 停止呢？--> 走到matrix边上时停止

### Solution:

```Java
class Solution {
    public int findCircleNum(int[][] isConnected) {
        if (isConnected == null) return 0;
        int length = isConnected.length;
        if (length == 0) return 0;
        
        
        int count = 0;
        for (int x = 0; x < length; x++) {
             if (isConnected[x][x] == 1) { //特别注意：为什么不从isConnected[x][0]开始呢？
                 count++;
                 dfs(isConnected, length, x);
            }
        }
           
        return count;
        
    }
    
    
    private void dfs(int[][] isConnected, int length, int curr) {
        if (curr >= length) return;
        for (int y = 0; y < length; y++) {
            if (isConnected[curr][y] == 1) {
                isConnected[curr][y] = 0;
                dfs(isConnected, length, y);
            }
            
        }
    }
}
```





