## 题目地址

https://leetcode-cn.com/problems/merge-two-sorted-lists/

## 题目描述
```
实现获取 下一个排列 的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。
如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。
必须 原地 修改，只允许使用额外常数空间。

示例 1：
输入：nums = [1,2,3]
输出：[1,3,2]

示例 2：
输入：nums = [3,2,1]
输出：[1,2,3]

示例 3：
输入：nums = [1,1,5]
输出：[1,5,1]

示例 4：
输入：nums = [1]
输出：[1]
 

提示：
1 <= nums.length <= 100
0 <= nums[i] <= 100

```

## 题目分析
```
注意到下一个排列总是比当前排列要大，除非该排列已经是最大的排列。我们希望找到一种方法，能够找到一个大于当前序列的新序列，
且变大的幅度尽可能小。
具体地，我们这样描述该算法，对于长度为n的排列a：

首先从后向前查找第一个顺序对 (i,i+1)，满足a[i]<a[i+1]。这样「较小数」即为 a[i]。此时[i+1,n)必然是下降序列。
如果找到了顺序对，那么在区间[i+1,n)中从后向前查找第一个元素 j 满足 a[i] < a[j]。这样「较大数」即为a[j]。
交换a[i]与 a[j]，此时可以证明区间 [i+1,n) 必为降序。我们可以直接使用双指针反转区间 [i+1,n) 使其变为升序，而无需对该区间进行排序。

如果在步骤 1 找不到顺序对，说明当前序列已经是一个降序序列，即最大的序列，我们直接跳过步骤 2 执行步骤 3，即可得到最小的升序序列。
```

## 代码实现
```
    public void nextPermutation(int[] nums) {
        int i = nums.length -2;
        while(i >= 0 && nums[i] >= nums[i+1]){
            i --;
        }
        if(i >= 0){
            int j = nums.length - 1;
            while(j >= 0 && nums[i] >= nums[j]){
                j--;
            }
            swap(nums,i,j);
        }
        reverse(nums,i+1);
    }
    public void swap(int[] nums,int i,int j){
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
    }
    public void reverse(int[] nums,int index){
        int left = index,right = nums.length - 1;
        while(left < right){
            swap(nums,left,right);
            left ++;
            right --;
        }
    }
```

## 复杂度分析
- 时间复杂度：O(n)
- 空间复杂度：O(n)