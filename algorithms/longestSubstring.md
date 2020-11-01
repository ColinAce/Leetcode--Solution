## 题目地址

https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/

## 题目描述
```
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

示例 2:
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

示例 3:
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

```

## 题目分析
```
我们使用两个指针表示字符串中的某个子串（的左右边界）。其中左指针代表着上文中「枚举子串的起始位置」，而右指针即为上文中的 r_k；

在每一步的操作中，我们会将左指针向右移动一格，表示 我们开始枚举下一个字符作为起始位置，然后我们可以不断地向右移动右指针，但需要保证这两个指针对应的子串中没有重复的字符。在移动结束后，这个子串就对应着 以左指针开始的，不包含重复字符的最长子串。我们记录下这个子串的长度；

在枚举结束后，我们找到的最长的子串的长度即为答案。

```

## 代码
```
public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        Set<Character> map = new HashSet<Character>();
        int right = -1,res = 0;
        for(int i = 0;i < n;i++){
            if(i != 0){
                map.remove(s.charAt(i-1));
            }
            while(right + 1 < n && !map.contains(s.charAt(right + 1))){
                map.add(s.charAt(right + 1));
                right++;
            }
            res = Math.max(res,right - i + 1);
        }
        return res;
    }
```