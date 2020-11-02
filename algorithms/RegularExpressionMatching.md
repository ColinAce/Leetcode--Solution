## 题目地址

https://leetcode-cn.com/problems/regular-expression-matching/

## 题目描述
```
给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '*' 的正则表达式匹配。

'.' 匹配任意单个字符
'*' 匹配零个或多个前面的那一个元素
所谓匹配，是要涵盖 整个 字符串 s的，而不是部分字符串。

示例 1：
输入：s = "aa" p = "a"
输出：false
解释："a" 无法匹配 "aa" 整个字符串。

示例 2:
输入：s = "aa" p = "a*"
输出：true
解释：因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。

示例 3：
输入：s = "ab" p = ".*"
输出：true
解释：".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。

示例 4：
输入：s = "aab" p = "c*a*b"
输出：true
解释：因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。

示例 5：
输入：s = "mississippi" p = "mis*is*p*."
输出：false
```

## 题目分析
```
动态规划:
    题目中的匹配是一个「逐步匹配」的过程：我们每次从字符串 p 中取出一个字符或者「字符 + 星号」的组合，并在 s 中进行匹配。对于 p 中一个字符而言，它只能在 s 中匹配一个字符，匹配的方法具有唯一性；而对于 p 中字符 + 星号的组合而言，它可以在 s 中匹配任意自然数个字符，并不具有唯一性。因此我们可以考虑使用动态规划，对匹配的方案进行枚举。

    字母 + 星号的组合在匹配的过程中，本质上只会有两种情况：

        匹配 s 末尾的一个字符，将该字符扔掉，而该组合还可以继续进行匹配；
        不匹配字符，将该组合扔掉，不再进行匹配。

    如果按照这个角度进行思考，我们可以写出很精巧的状态转移方程：

    f[i][j] = { f[i−1][j] or f[i][j−2],     s[i]=p[j−1]
                f[i][j−2],                  s[i]!=p[j−1]  }

    在任意情况下，只要 p[j] 是 .，那么 p[j] 一定成功匹配 s 中的任意一个小写字母。
    最终的状态转移方程如下：
        f[i][j]={
            if (p[j] != ‘*’)={f[i−1][j−1],matches(s[i],p[j])
                                false,otherwise}	
            otherwise = {f[i-1][j] or f[i][j-2],matches(s[i],p[j-1])
                        f[i][j-2],otherwise}
            }
        } 
    其中 matches(x,y) 判断两个字符是否匹配的辅助函数。只有当 y 是 . 或者 x 和 y 本身相同时，这两个字符才会匹配。

​	
```

## 代码
```
public boolean isMatch(String s,String p) {
		int m = s.length();
		int n = p.length();
		
		boolean[][] dp = new boolean[m+1][n+1];
		dp[0][0] = true;
		for(int i = 0;i <= m; i++) {
			for(int j = 1;j <= n;j++) {
				if(p.charAt(j-1) == '*') {
					dp[i][j] = dp[i][j-2];
					if(matches(s,p,i,j-1)) {
						dp[i][j] = dp[i][j] || dp[i-1][j];
					}
				}
				else {
					if(matches(s,p,i,j)) {
						dp[i][j] = dp[i-1][j-1];
					}
				}
			}
		}
		return dp[m][n];
	}
	public boolean matches(String s,String p,int i,int j) { //判断两个字符是否匹配
        char[] sCharArray = s.toCharArray();
            char[] pCharArray = p.toCharArray();
		if(i == 0) {
			return false;
		}
		if(pCharArray[j - 1] == '.') {
			return true;
		}
		return sCharArray[i - 1] == pCharArray[j - 1];
	}
```

## 复杂度分析
- 时间复杂度：O(mn)
- 空间复杂度：O(mn)