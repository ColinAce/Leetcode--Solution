## 题目地址

https://leetcode-cn.com/problems/container-with-most-water/submissions/

## 题目描述
```
给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 
和 (i, 0) 。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器。
```
![](https://github.com/ColinAce/Leetcode--Solution/blob/master/pic/question_11.jpg)

```
示例1：
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

示例 2：
输入：height = [1,1]
输出：1

示例 3：
输入：height = [4,3,2,1,4]
输出：16

示例 4：
输入：height = [1,2,1]
输出：2
```


## 题目分析
```
双指针：
    本题是一道经典的面试题，最优的做法是使用「双指针」。
    双指针代表的是 可以作为容器边界的所有位置的范围。在一开始，双指针指向数组的左右边界，表示数组中所有的位置都可以作为容器的边界，
    因为我们还没有进行过任何尝试。在这之后，我们每次将 对应的数字较小的那个指针往另一个指针的方向移动一个位置，就表示我们认为这个
    指针不可能再作为容器的边界了。

    这样一来，我们将问题的规模减小了1，被我们丢弃的那个位置就相当于消失了。此时的左右指针，就指向了一个新的、规模减少了的问题的数组
    的左右边界，因此，我们可以继续像之前考虑第一步那样考虑这个问题：
        求出当前双指针对应的容器的容量；
        对应数字较小的那个指针以后不可能作为容器的边界了，将其丢弃，并移动对应的指针。
```


## 代码实现
```
public int maxArea(int[] height) {
        int left = 0,right = height.length - 1;
        int ans = 0;
        while(left<right){
            int temp = Math.min(height[left],height[right]) * (right - left);
            ans = Math.max(ans,temp);
            if(height[left] > height[right])    --right;
            else    ++left;
        }
        return ans;
    }
```

## 复杂度分析
- 时间复杂度：O(n)   遍历一遍数组
- 空间复杂度：O(1)