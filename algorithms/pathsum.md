## 题目地址

https://leetcode-cn.com/problems/path-sum/

## 题目描述
```
给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

说明: 叶子节点是指没有子节点的节点。

示例: 
给定如下二叉树，以及目标和 sum = 22，

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1


返回 true, 因为存在目标和为 22 的根节点到叶子节点的路径 5->4->11->2。

```

## 前置知识
- 广度优先搜索
- 递归
- 回溯算法

## 递归思路

递归：假定从根节点到当前节点的值之和为 val，我们可以将这个大问题转化为一个小问题：是否存在从当前节点的子节点到叶子的路径，满足其路径和为 sum - val。这满足递归的性质，若当前节点就是叶子节点，那么我们直接判断 sum 是否等于 val 即可（因为路径和已经确定，就是当前节点的值，我们只需要判断该路径和是否满足条件）。若当前节点不是叶子节点，我们只需要递归地询问它的子节点是否能满足条件即可。

## 代码实现
```

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool hasPathSum(TreeNode *root, int sum) {
        if(root == nullptr){
            return false;
        }
        if(root->left == nullptr && root->right == nullptr){
            return sum == root->val;
        }
        return hasPathSum(root->left,sum-root->val)||hasPathSum(root->right,sum-root->val);
    }
};

```
**复杂度分析**

- 时间复杂度：O(N)，对每个节点访问一次
- 空间复杂度：O(log⁡N)，空间复杂度主要取决于递归时栈空间的开销，最坏情况下，树呈现链状，空间复杂度为 O(N)。平均情况下树的高度与节点数的对数正相关，空间复杂度为 O(log⁡N)。

## BFS思路

使用广度优先搜索的方式，记录从根节点到当前节点的路径和，以防止重复计算。
这样我们使用两个队列，分别存储将要遍历的节点，以及根节点到这些节点的路径和即可。

## BFS代码实现

```
class Solution {
public:
    bool hasPathSum(TreeNode *root, int sum) {
        if (root == nullptr) {
            return false;
        }
        queue<TreeNode *> que_node;
        queue<int> que_val;
        que_node.push(root);
        que_val.push(root->val);
        while (!que_node.empty()) {
            TreeNode *now = que_node.front();
            int temp = que_val.front();
            que_node.pop();
            que_val.pop();
            if (now->left == nullptr && now->right == nullptr) {
                if (temp == sum) return true;
                continue;
            }
            if (now->left != nullptr) {
                que_node.push(now->left);
                que_val.push(now->left->val + temp);
            }
            if (now->right != nullptr) {
                que_node.push(now->right);
                que_val.push(now->right->val + temp);
            }
        }
        return false;
    }
};

```

**复杂度分析**

- 时间复杂度：O(N)，对每个节点访问一次
- 空间复杂度：O(N)，其中N是树的节点数。空间复杂度主要取决于队列的开销，队列中的元素个数不会超过树的节点数。