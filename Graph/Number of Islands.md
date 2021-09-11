### 200. Number of Islands

Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

![image](https://user-images.githubusercontent.com/75393672/132940271-9f282484-d660-4b04-9c87-f6bc0f543863.png)

### 问题：
1. 如何表示edges
2. 如何表示穷尽了一个island

### Solution:

```Java
class Solution {
    public int numIslands(char[][] grid) {
        if (grid.length == 0) return 0;
        int count = 0;
        for (int y = 0; y < grid.length; y++) {
            for (int x = 0; x < grid[0].length; x++) {
                if (grid[y][x] == '1') {
                    count++;
                    dfs(grid, x, y); //用dfs的方式穷尽island
                }
            }
        }
        return count;
    }
    private void dfs(char[][] grid, int x, int y) {
        if (x >= grid[0].length || y >= grid.length || x < 0 || y < 0 || grid[y][x] == '0') {
            return; //这里一定要大于等于，因为等于的时候就说明已经到了最后一个格子，需要return
        }
        grid[y][x] = '0'; //eat the islands, or it will be double counted 这段很重要
        dfs(grid, x + 1, y);
        dfs(grid, x - 1, y);
        dfs(grid, x, y + 1);
        dfs(grid, x, y - 1);
    }
}
```


