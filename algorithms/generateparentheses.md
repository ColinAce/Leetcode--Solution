## 题目地址

https://leetcode-cn.com/problems/merge-two-sorted-lists/

## 题目描述
```
数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。

示例：
输入：n = 3
输出：[
       "((()))",
       "(()())",
       "(())()",
       "()(())",
       "()()()"
     ]
```

## 题目分析
```
暴力法：
    我们可以生成所有 2^2n个 '(' 和 ')' 字符构成的序列，然后我们检查每一个是否有效即可。

    算法
    为了生成所有序列，我们可以使用递归。长度为 n 的序列就是在长度为 n-1 的序列前加一个 '(' 或 ')'。
    为了检查序列是否有效，我们遍历这个序列，并使用一个变量 balance 表示左括号的数量减去右括号的数量。如果在遍历过程中 balance 的值小于
    零，或者结束时 balance 的值不为零，那么该序列就是无效的，否则它是有效的。

回溯法：

    我们可以只在序列仍然保持有效时才添加 '(' or ')'，而不是像 方法一 那样每次添加。我们可以通过跟踪到目前为止放置的左括号和右括号的数目
    来做到这一点，
    如果左括号数量不大于 n，我们可以放一个左括号。如果右括号数量小于左括号的数量，我们可以放一个右括号。

```
## 算法
```
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList<String>();
        backtrack(ans, new StringBuilder(), 0, 0, n);
        return ans;
    }

    public void backtrack(List<String> ans, StringBuilder cur,int left, int right,int num){
        if(cur.length() == num * 2){
            ans.add(cur.toString());
            return;
        }
        if(left < num){
            cur.append('(');
            backtrack(ans, cur, left + 1, right, num);
            cur.deleteCharAt(cur.length() - 1);
        }
        if(left > right){
            cur.append(')');
            backtrack(ans, cur, left, right + 1, num);
            cur.deleteCharAt(cur.length() - 1);
        }
    }
```