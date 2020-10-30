## 题目地址

https://leetcode-cn.com/problems/longest-palindromic-substring/

## 题目描述
```
给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

示例 1：
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。

示例 2：
输入: "cbbd"
输出: "bb"
```

## 题目分析
```
方法一：动态规划
    对于一个子串而言，如果它是回文串，并且长度大于 2，那么将它首尾的两个字母去除之后，它仍然是个回文串。例如对于字符串“ababa”，如果我们已经知道 “bab” 是回文串，那么 “ababa” 一定是回文串，这是因为它的首尾两个字母都是 “a”。

    根据这样的思路，我们就可以用动态规划的方法解决本题。我们用 P(i,j) 表示字符串 s 的第 i 到 j 个字母组成的串（下文表示成 s[i:j]）是否为回文串：
    P(i,j)={ true,如果子串 S_i... S_j是回文串
             false,如果子串 S_i...S_j不是回文串 }

    其它情况
    这里的「其它情况」包含两种可能性：
    s[i, j]本身不是一个回文串；i>j，此时 s[i,j] 本身不合法。

    那么我们就可以写出动态规划的状态转移方程：
    P(i,j)=P(i+1,j−1)∧(S_i ==S_j)

    也就是说，只有 s[i+1:j-1] 是回文串，并且 s 的第 i 和 j 个字母相同时，s[i:j] 才会是回文串。

    上文的所有讨论是建立在子串长度大于 2 的前提之上的，我们还需要考虑动态规划中的边界条件，即子串的长度为 1 或 2。对于长度为 1 的子串，它显然是个回文串；对于长度为 2 的子串，只要它的两个字母相同，它就是一个回文串。因此我们就可以写出动态规划的边界条件：
    P(i,i)=true
    P(i,i+1)=(S_i==S_i+1)

    根据这个思路，我们就可以完成动态规划了，最终的答案即为所有 P(i,j)=true 中 j−i+1（即子串长度）的最大值。注意：在状态转移方程中，我们是从长度较短的字符串向长度较长的字符串进行转移的，因此一定要注意动态规划的循环顺序。

方法二: 中心扩散法
    状态转移链：
    P(i,j)←P(i+1,j−1)←P(i+2,j−2)←⋯←某一边界情况

    可以发现，所有的状态在转移的时候的可能性都是唯一的。也就是说，我们可以从每一种边界情况开始「扩展」，也可以得出所有的状态对应的答案。
    边界情况即为子串长度为 1 或 2 的情况。我们枚举每一种边界情况，并从对应的子串开始不断地向两边扩展。如果两边的字母相同，我们就可以继续扩展，
    例如从 P(i+1,j−1) 扩展到 P(i,j)；如果两边的字母不同，我们就可以停止扩展，因为在这之后的子串都不能是回文串了。

    方法二的本质即为：我们枚举所有的「回文中心」并尝试「扩展」，直到无法扩展为止，此时的回文串长度即为此「回文中心」下的最长回文串长度。我们对
    所有的长度求出最大值，即可得到最终的答案。

```

##  代码实现
```
动态规划：
public class longestPalindrome {
	
	//动态规划方法  P(i,j)=P(i+1,j−1)∧(Si ==Sj)
	public String longestPalindrome(String s) {
        int length = s.length();
        String res = new String();
        boolean[][] dp = new boolean[length][length];
        for(int dis = 0;dis < length;dis++) {
        	for(int i = 0;i+dis < length;i++) {
        		int j = i + dis;
        		if(dis == 0) {//i=j时
        			dp[i][j] = true;
        		}
        		else if(dis == 1) {//i、j相差为1时
        			dp[i][j] = (s.charAt(i) == s.charAt(j));
        		}
        		else {
        			dp[i][j] = (s.charAt(i) == s.charAt(j))&&dp[i+1][j-1];
        		}
        		if(dp[i][j] && dis+1 > res.length()) {
        			res = s.substring(i,i + dis + 1);
        		}
        	}
        }
        return res;
    }
```

```
中心扩散：
    //中心扩散法 ：遍历每个回文字符串的中心，向两边扩散
	public String longestPalindrome2(String s) {
		int n = s.length();
		if(s == null || n < 1) {
			return "";
		}
		int start = 0,end = 0;
		for(int i = 0;i < n;i++) {
			int len1 = Expand(s,i,i); //以单个元素为中心的扩展
			int len2 = Expand(s,i,i+1); //以双元素为中心扩展
			int len = Math.max(len1, len2);
			if(len > end - start + 1) {
				start = i - (len - 1)/2;
				end = i+ len/2;
			}
		}
		return s.substring(start, end + 1);
	}
	public int Expand(String s,int left,int right) {
		while(left >= 0 && right < s.length() && s.charAt(left)==s.charAt(right)) {
			--left;
			++right;
		}
		return right - left - 1;
	}
}
```

## 复杂度分析
- 动态规划
- 时间复杂度：O(n^2)
- 空间复杂度：O(n^2)

- 中心扩散
- 时间复杂度：O(n^2)，其中n是字符串的长度。长度为1和2的回文中心分别有n和n−1个，每个回文中心最多会向外扩展O(n)次。
- 空间复杂度：O(1)

