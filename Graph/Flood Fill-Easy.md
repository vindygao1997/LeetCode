### 7.. Flood Fill

An image is represented by an m x n integer grid image where image[i][j] represents the pixel value of the image.

You are also given three integers sr, sc, and newColor. You should perform a flood fill on the image starting from the pixel image[sr][sc].

To perform a flood fill, consider the starting pixel, plus any pixels connected 4-directionally to the starting pixel of the same color as the starting pixel, plus any pixels connected 4-directionally to those pixels (also with the same color), and so on. Replace the color of all of the aforementioned pixels with newColor.

Return the modified image after performing the flood fill.

![image](https://user-images.githubusercontent.com/75393672/132976111-bc4019a2-bf2e-49cc-8e7a-04ae41f6d3ae.png)

### 问题：

1. 如何保证flood的时候只flood image[x][y] == starting color? -> 把image[x][y]!= starting color作为一个return条件
2. 如何保证所有地方都被Flood到？ -> dfs

### Solution:

```Java

class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        if (image[sr][sc] == newColor) return image;//很重要！一开始漏掉了就stackOverflow了
        int oldColor = image[sr][sc];
        flooding(image, sr, sc, oldColor, newColor);
 
        return image;
    }
    private void flooding(int[][] image, int x, int y, int oldColor, int newColor) {
        int m = image.length; //image.length对左起第一个格子
        int n = image[0].length;//image[0].length对左起第二个格子
        if (x >= m || y >= n || x < 0 || y < 0 || image[x][y] != oldColor) return; //原来不是oldColor的地方不要Dfs //>=很重要！总是会写成>
        image[x][y] = newColor;//记得改成newColor
        flooding(image, x + 1, y, oldColor, newColor);
        flooding(image, x - 1, y, oldColor, newColor);
        flooding(image, x, y + 1, oldColor, newColor);
        flooding(image, x, y - 1, oldColor, newColor);
    }
}
            
```

