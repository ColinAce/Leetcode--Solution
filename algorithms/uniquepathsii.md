## 题目地址

https://leetcode-cn.com/problems/unique-paths-ii/

## 题目描述

```
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。
机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。
现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？
网格中的障碍物和空位置分别用 1 和 0 来表示。
说明：m 和 n 的值均不超过 100。
```
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/robot_maze.png)

## 前置知识

- 动态规划

## 思路

典型的动态规划题型，它和爬楼梯、背包等问题都属于动态规划肿较简单的问题，经常出现在面试之中。
假设我们定义到达右下角的走法数为 f(m,n), 因为右下角只能由它上方或者左方的格子走过去，因此可以很容易的写出递归求解式，即 f(m,n)=f(m−1,n)+f(m,n−1)，最后加上递归终止条件。二维数组中自顶向下的递推，即 dp[i,j]=dp[i−1,j]+dp[i,j−1]。

### 1、状态定义

dp[i][j] 表示走到格子 (i,j)的路径数量。

### 2、状态转移

如果网格 (i,j)上有障碍物，则 dp[i][j]值为0，表示走到该格子的方法数为0；
否则网格 (i,j)可以从网格 (i−1,j)或者 网格 (i,j−1)走过来，因此走到该格子的方法数为走到网格 (i−1,j)和网格 (i,j−1)的方法数之和，即 dp[i,j]=dp[i−1,j]+dp[i,j−1]。

### 3、初始条件

第 1 列的格子只有从其上边格子走过去这一种走法，因此初始化 dp[i][0] 值为 1，存在障碍物时为 0；
第 1 行的格子只有从其左边格子走过去这一种走法，因此初始化 dp[0][j] 值为 1，存在障碍物时为 0。

## 代码实现

C++：

```
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int row = obstacleGrid.size();
        int col = obstacleGrid[0].size();
        vector<vector<int>> dp(row,vector<int>(col,0));

        for(int i=0;i<col;i++){
            if(obstacleGrid[0][i]) break;
            else dp[0][i] = 1;
        }
        for(int i=0;i<row;i++){
            if(obstacleGrid[i][0]) break;
            else dp[i][0] = 1;
        }

        for(int i=1;i<row;i++){
            for(int j=1;j<col;j++){
                if(!obstacleGrid[i][j])
                dp[i][j] = dp[i][j-1] + dp[i-1][j];
            }
        }
        return dp[row-1][col-1];
    }
};
```

**复杂度分析**

- 时间复杂度：O(M * N)
- 空间复杂度：O(M * N)

## 空间优化

我们可以运用「滚动数组思想」把空间复杂度优化成O(m)。「滚动数组思想」是一种常见的动态规划优化方法，当我们定义的状态在动态规划的转移方程中只和某几个状态相关的时候，就可以考虑这种优化方法，目的是给空间复杂度「降维」。

经过优化之后的代码为：
```
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int n = obstacleGrid.size(), m = obstacleGrid.at(0).size();
        vector <int> f(m);

        f[0] = (obstacleGrid[0][0] == 0);
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (obstacleGrid[i][j] == 1) {
                    f[j] = 0;
                    continue;
                }
                if (j - 1 >= 0 && obstacleGrid[i][j - 1] == 0) {
                    f[j] += f[j - 1];
                }
            }
        }

        return f.back();
    }
};

```