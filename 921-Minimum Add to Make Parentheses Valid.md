# 921. 使括号有效的最少添加

## 题目
给定一个由 '(' 和 ')' 括号组成的字符串 S，我们需要添加最少的括号（ '(' 或是 ')'，可以在任何位置），以使得到的括号字符串有效。

从形式上讲，只有满足下面几点之一，括号字符串才是有效的：

它是一个空字符串，或者
它可以被写成 AB （A 与 B 连接）, 其中 A 和 B 都是有效字符串，或者
它可以被写作 (A)，其中 A 是有效字符串。
给定一个括号字符串，返回为使结果字符串有效而必须添加的最少括号数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/minimum-add-to-make-parentheses-valid

## Python
```
class Solution:
    def minAddToMakeValid(self, S: str) -> int:
        while '()' in S:
            S = S.replace('()', '')

        return len(S)
```

## C++
```
class Solution {
public:
    int minAddToMakeValid(string S) {
        // 以左括号为基准
        int left_need = 0;
	int right_need = 0;
        for(char c : S) {
            if(c == '(')
                right_need++;
            else if(c == ')')
                right_need--;
            
            if(right_need == -1) {
                right_need = 0;
                left_need++;
            }
        }

        return left_need + right_need;

    }
};
```
