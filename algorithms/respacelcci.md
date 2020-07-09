## 题目地址

https://leetcode-cn.com/problems/re-space-lcci/

## 题目描述
```
哦，不！你不小心把一个长篇文章中的空格、标点都删掉了，并且大写也弄成了小写。像句子"I reset the computer. It still didn’t boot!"
已经变成了"iresetthecomputeritstilldidntboot"。在处理标点符号和大小写之前，你得先把它断成词语。当然了，你有一本厚厚的词典dictionary，
不过，有些词没在词典里。假设文章用sentence表示，设计一个算法，把文章断开，要求未识别的字符最少，返回未识别的字符数。

注意：本题相对原题稍作改动，只需返回未识别的字符数

示例：

输入：
dictionary = ["looked","just","like","her","brother"]
sentence = "jesslookedjustliketimherbrother"
输出： 7
解释： 断句后为"jess looked just like tim her brother"，共7个未识别字符。

提示：


	0 <= len(sentence) <= 1000
	dictionary中总字符数不超过 150000。
	你可以认为dictionary和sentence中只包含小写字母。
```

## 前置知识
- 动态规划

## 思路
假设前i个字符的最少未匹配数为x，计算前i+1个字符的最少未匹配数。如果第i+1个字符加入后前i+1个字符肿从某个字符开始到第i+1个字符组成的字符串刚好与dictionary中
某单词匹配，则前i+1个字符的最少未匹配数为那个字符之前的字符串的最少未匹配数，否则为前i个字符的最少未匹配数+1。假设那个字符下标为idx，则dp[i] = min(dp[i],dp[idx])。


### 1、状态定义

dp[i]表示前i个字符的最少未匹配数

### 2、状态转移

假设当前我们已经考虑完了前i个字符了，对于前i + 1个字符对应的最少未匹配数：

第i + 1个字符未匹配，则dp[i + 1]=dp[i] + 1，即不匹配数加1;
遍历前i个字符，若以其中某一个下标idx为开头、以第i + 1个字符为结尾的字符串正好在词典里，则 dp[i] = min(dp[i], dp[idx]) 更新 dp[i]。


## 3、初始条件

dp[0] = 0

## 代码实现

C++

```

class Solution {
public:
    int respace(vector<string>& dictionary, string sentence) {
        if(sentence.length()==0) return 0;
        int n = sentence.length();
        if(dictionary.size()==0) return n;
        vector<int> dp(n+1);
        for(int i=1;i<=n;i++){
            dp[i]=dp[i-1]+1;
            for(string word:dictionary){
                if(word.size()<=i&&word==sentence.substr(i-word.size(),word.size()))
                dp[i]=min(dp[i],dp[i-word.size()]);
            }
        }
        return dp[n];
    }
};

```
**复杂度分析**

- 时间复杂度：O(N * M)
- 空间复杂度：O(N)