## 题目地址

https://leetcode-cn.com/problems/dungeon-game/

## 题目描述
```
一些恶魔抓住了公主（P）并将她关在了地下城的右下角。地下城是由 M x N 个房间组成的二维网格。我们英勇的骑士（K）最初被安置在左上角的房间里，
他必须穿过地下城并通过对抗恶魔来拯救公主。
骑士的初始健康点数为一个正整数。如果他的健康点数在某一时刻降至 0 或以下，他会立即死亡。
有些房间由恶魔守卫，因此骑士在进入这些房间时会失去健康点数（若房间里的值为负整数，则表示骑士将损失健康点数）；其他房间要么是空的（房间里的
值为 0），要么包含增加骑士健康点数的魔法球（若房间里的值为正整数，则表示骑士将增加健康点数）。
为了尽快到达公主，骑士决定每次只向右或向下移动一步。
编写一个函数来计算确保骑士能够拯救到公主所需的最低初始健康点数。

例如，考虑到如下布局的地下城，如果骑士遵循最佳路径 右 -> 右 -> 下 -> 下，则骑士的初始健康点数至少为 7。
 -2(K) -3    3
  -5   -10   1
  10    30 -5(P)

说明:
	骑士的健康点数没有上限。
	任何房间都可能对骑士的健康点数造成威胁，也可能增加骑士的健康点数，包括骑士进入的左上角房间以及公主被监禁的右下角房间。
```

## 前置知识

- 动态规划

## 思路
走方格的题目很容易让人想起动态规划的算法，按照经验从左上往右下的顺序进行动态规划，对于每一条路径，需要同时记录两个值，第一个是「从出发点到当前点的路径和」，第二个是「从出发点到当前点所需的最小初始值」，我们无法直接确定到达从出发点到达当前点的路径方案，，因为有两个重要程度相同的参数同时影响后续的决策。也就是说，这样的动态规划是不满足「无后效性」的。
于是我们考虑从右下往左上进行动态规划。令dp[i][j]表示从坐标(i,j)到终点所需的最小初始值。换句话说，当我们到达坐标(i,j)时，如果此时我们的路径和不小于 dp[i][j]，我们就能到达终点。
这样一来，我们就无需担心路径和的问题，只需要关注最小初始值。对于dp[i][j]，我们只要关心dp[i][j+1]和dp[i+1][j]的最小值 minn。记当前格子的值为 dungeon(i,j)，那么在坐标(i,j)的初始值只要达到minn−dungeon(i,j) 即可。同时，初始值还必须大于等于1。这样我们就可以得到状态转移方程：
dp[i][j]=max⁡(min⁡(dp[i+1][j],dp[i][j+1])−dungeon(i,j),1)
最终答案即为 dp[0][0]。
### 1、状态定义

dp[i][j]表示从坐标(i,j)到终点所需的最小初始值。

### 2、状态转移

dp[i][j]=max⁡(min⁡(dp[i+1][j],dp[i][j+1])−dungeon(i,j),1)

### 3、初始条件

边界条件为，当i=n−1 或者j=m−1 时，dp[i][j] 转移需要用到的 dp[i][j+1] 和 dp[i+1][j] 中有无效值，因此代码实现中给无效值赋值为极大值。特别地，dp[n−1][m−1] 转移需要用到的 dp[n−1][m] 和 dp[n][m−1] 均为无效值，因此我们给这两个值赋值为1。

## 代码实现

C++：
```
class Solution {
public:
    int calculateMinimumHP(vector<vector<int>>& dungeon) {
        int n = dungeon.size(),m = dungeon[0].size();
        vector<vector<int>> dp(n+1,vector<int>(m+1,INT_MAX));
        dp[n][m-1] = 1,dp[n-1][m] = 1;
        for(int i=n-1;i>=0;i--){
            for(int j=m-1;j>=0;j--){
                dp[i][j] = max(min(dp[i][j+1],dp[i+1][j])-dungeon[i][j],1);
            }
        }
         return dp[0][0];
    }
};
```
**复杂度分析**

- 时间复杂度：O(N×M)，其中 N,M为给定矩阵的长宽。
- 空间复杂度：O(N×M)，其中 N,M 为给定矩阵的长宽，注意这里可以利用滚动数组进行优化，优化后空间复杂度可以达到 O(N)。

## 空间优化
可以发现，某个值只和右边一列和本列有关系，所以可以空间优化

C++:
```
class Solution {
    public static void main(String[] args) {
        System.out.println(new Solution().calculateMinimumHP(new int[][]{{-2, -3, 3}, {-5, -10, 1}, {10, 30, -5}}));
    }
    public int calculateMinimumHP(int[][] dungeon) {
        int m = dungeon.length, n = dungeon[0].length;
        int[] dp = new int[n];
        dp[n - 1] = Math.max(1, 1 - dungeon[m - 1][n - 1]);
        for (int i = n - 2; i >= 0; i--) dp[i] = Math.max(1, dp[i + 1] - dungeon[m - 1][i]);
        for (int i = m - 2; i >= 0; i--) {
            dp[n - 1] = Math.max(1, dp[n - 1] - dungeon[i][n - 1]);
            for (int j = n - 2; j >= 0; j--) {
                dp[j]=Math.max(1,Math.min(dp[j]-dungeon[i][j],dp[j+1]-dungeon[i][j]));
            }
        }
        return dp[0];
    }
}
```