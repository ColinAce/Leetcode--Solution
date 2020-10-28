## 题目地址

https://leetcode-cn.com/problems/median-of-two-sorted-arrays/solution/xun-zhao-liang-ge-you-xu-shu-zu-de-zhong-wei-s-114/

## 题目描述
```
给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的中位数。

进阶：你能设计一个时间复杂度为 O(log (m+n)) 的算法解决此问题吗？


示例 1：
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2

输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
```

## 题目分析
```
自然想到归并方式，将两个数组合并为一个递增数组，然后求中值。或者在两个数组中找到第(m+n)/2小的数。但这两种方法的时间复杂度都为O(m+n)，
不符合题目要求。
一般时间复杂度为O(log(m+n))时的查找算法会联想到二分查找，也叫折半查找。

根据中位数的定义，当 m+nm+n 是奇数时，中位数是两个有序数组中的第 (m+n)/2个元素，当 m+n 是偶数时，中位数是两个有序数组中的第 (m+n)/2 
个元素和第 (m+n)/2+1 个元素的平均值。因此，这道题可以转化成寻找两个有序数组中的第 k 小的数，其中 k 为 (m+n)/2 或 (m+n)/2+1。

假设两个有序数组分别是 A 和 B。要找到第 k 个元素，我们可以比较 A[k/2−1] 和 B[k/2−1]，其中 / 表示整数除法。由于 A[k/2−1] 和 B[k/2−1]
的前面分别有 A[0..k/2−2] 和 B[0..k/2−2]，即 k/2-1 个元素，对于 A[k/2−1] 和 B[k/2−1] 中的较小值，最多只会有 (k/2-1)+(k/2-1)≤k−2 个
元素比它小，那么它就不能是第 k 小的数了，则可以直接舍弃它和它之前的数。舍弃之后即为寻找两个数组的第 k-i(i为舍弃的数的数量)个元素。
可分为三种情况：
    如果 A[k/2−1]<B[k/2−1]，则比 A[k/2−1] 小的数最多只有 A 的前 k/2−1 个数和 B 的前 k/2−1 个数，即比 A[k/2−1] 小的数最多只有 k−2 个，
    因此 A[k/2−1] 不可能是第 k 个数，A[0] 到 A[k/2−1] 也都不可能是第 k 个数，可以全部排除。

    如果 A[k/2−1]>B[k/2−1]，则可以排除 \text{B}[0]B[0] 到 B[k/2−1]。

    如果 A[k/2−1]=B[k/2−1]，则可以归入第一种情况处理。

可以看到，比较 A[k/2−1] 和 B[k/2−1] 之后，可以排除 k/2 个不可能是第 k 小的数，查找范围缩小了一半。同时，我们将在排除后的新数组上继续进
行二分查找，并且根据我们排除数的个数，减少 k 的值，这是因为我们排除的数都不大于第 k 小的数。

有以下三种情况需要特殊处理：

    如果 A[k/2−1] 或者 B[k/2−1] 越界，那么我们可以选取对应数组中的最后一个元素。在这种情况下，我们必须根据排除数的个数减少 k 的值，而不
    能直接将 k 减去 k/2。

    如果一个数组为空，说明该数组中的所有元素都被排除，我们可以直接返回另一个数组中第 k 小的元素。

    如果 k=1，我们只要返回两个数组首元素的最小值即可。
```

## 代码实现
```
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int length1 = nums1.length,length2 = nums2.length;
        int sum = length1 + length2;
        if(sum % 2 == 1 ){
            int mid = sum / 2;
            double median = getKthElement(nums1, nums2,mid + 1);
            return median;
        }
        else{
            int mid1 = sum / 2,mid2 = sum / 2 + 1;
            double median = (getKthElement(nums1, nums2,mid1) + getKthElement(nums1, nums2,mid2)) / 2.0;
            return median;
        }
        
    }

    
    public int getKthElement(int[] nums1,int[] nums2,int k){
        int length1 = nums1.length,length2 = nums2.length;
        int index1 = 0,index2 = 0;

        while(true){
            //边界情况
            if(index1 == length1){
                return nums2[index2 + k - 1];
            }
            if(index2 == length2){
                return nums1[index1 + k - 1];
            }
            if(k == 1){
                return Math.min(nums1[index1],nums2[index2]);
            }
            //一般情况
            int half = k / 2;
            int newIndex1 = Math.min(index1 + half,length1) - 1; //防止越界
            int newIndex2 = Math.min(index2 + half,length2) - 1;
            int pivot1 = nums1[newIndex1],pivot2 = nums2[newIndex2];
            if(pivot1 <= pivot2){
                k -=newIndex1-index1 + 1;//nums1中去掉newIndex1-index1 + 1个数
                index1 = newIndex1 + 1;
            }
            else{
                k -= newIndex2 - index2 + 1;
                index2 = newIndex2 + 1;
            }
        }
    }
}
```

## 复杂度分析
- 时间复杂度：O(log(m+n)),每一轮循环可以将查找范围减少一半，因此时间复杂度是O(log(m+n))。
- 空间复杂度：空间复杂度：O(1)O(1)。