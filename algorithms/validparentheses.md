## 题目地址

https://leetcode-cn.com/problems/valid-parentheses/

## 题目描述
```
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
        有效字符串需满足：
            左括号必须用相同类型的右括号闭合。
            左括号必须以正确的顺序闭合。
        注意空字符串可被认为是有效字符串。

示例 1:
输入: "()"
输出: true

示例 2:
输入: "()[]{}"
输出: true

示例 3:
输入: "(]"
输出: false

示例 4:
输入: "([)]"
输出: false

示例 5:
输入: "{[]}"
输出: true
```


## 题目分析
```
栈：
    括号匹配，很容易联想到数据结构中栈的应用，将每个左括号都压入栈中，当遇到右括号时，查看栈顶左括号是否可以与这个右括号匹配。
如果匹配，则进行消解(即将栈顶的左括号出栈)；如果不能匹配，则直接返回错误。当括号全部匹配完，如果栈为空，则说明是有效的括号，否
则返回错误。
```

## 代码实现
```
利用StringBuilder类模拟栈：
public boolean isValid(String s) {
	        StringBuilder stack = new StringBuilder();
	        int n = s.length();
	        
	        for(int i = 0;i < n;i ++){
	            char ch = s.charAt(i);
	            char Sch;
	            if(ch == '(' || ch == '[' || ch == '{'){
	                stack.append(ch);
	            }
	            else if(ch == ')'){
	            	if(stack.length()<=0) return false;
	            	else {
	            		Sch = stack.charAt(stack.length()-1);
		                if(Sch == '('){
		                    stack.deleteCharAt(stack.length()-1);
		                }
		                else return false;
	            	}
	                
	            }
	            else if(ch == ']'){
	            	if(stack.length()<=0) return false;
	            	else {
	            		Sch = stack.charAt(stack.length()-1);
		                if(Sch == '['){
		                    stack.deleteCharAt(stack.length()-1);
		                }
		                else return false;
	            	}
	                
	            }
	            else{
	            	if(stack.length()<=0) return false;
	            	else {
	            		Sch = stack.charAt(stack.length()-1);
		                if(Sch == '{'){
		                    stack.deleteCharAt(stack.length()-1);
		                }
		                else return false;
	            	}
	                
	            }
	        }
	        if(stack.length()<=0) {
	        	return true;
	        }
	        else {
	        	return false;
	        }
	    }
```

```
利用Java Deque类(双向队列)表示栈：
class Solution {
    public boolean isValid(String s) {
        int n = s.length();
        if (n % 2 == 1) {
            return false;
        }

        Map<Character, Character> pairs = new HashMap<Character, Character>() {{
            put(')', '(');
            put(']', '[');
            put('}', '{');
        }};
        Deque<Character> stack = new LinkedList<Character>();
        for (int i = 0; i < n; i++) {
            char ch = s.charAt(i);
            if (pairs.containsKey(ch)) {
                if (stack.isEmpty() || stack.peek() != pairs.get(ch)) {
                    return false;
                }
                stack.pop();
            } else {
                stack.push(ch);
            }
        }
        return stack.isEmpty();
    }
}

```