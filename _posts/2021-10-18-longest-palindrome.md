---
layout: post
title: "[LeetCode 100] 5. Longest Palindromic Substring"
subtitle: 
date: 2021-10-18
background: '/img/posts/code.jpg'
---
<h3>Problem:</h3>
<p>
Given a string s, return the longest palindromic substring in s.
</p>

```
Example 1:
Input: s = "babad"
Output: "bab"
Note: "aba" is also a valid answer.

Example 2:
Input: s = "cbbd"
Output: "bb"

Example 3:
Input: s = "a"
Output: "a"

Example 4:
Input: s = "ac"
Output: "a"
```

<br/>
<h3>Solution:</h3>
<p>1. Dynamic Programming 방법</p>
<p>
만약 Substring $S_{i+1},...S_{j-1}$가 Palindrome이고 $(i < j)$, $S_i==S_j$이면 $S_i,...,S_j$도 Palindrome이다. 즉,
<br/>
<center>$P(i,j)=(P(i+1,j-1)$ and $S_i==S_j)$</center>
</p>

<p>2. 더 빠른 방법</p>
<p>
모든 Palindrome을 찾을 필요없이 최대 길이의 Palindrome만 찾으면 되므로 기존에 찾은 Palindrome보다 길이가 더 긴 것이 존재하는 지만 체크하면 된다.
</p>

<br/>
<h3>Code:</h3>

<p>1. Dynamic Programming</p>

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        if len(s) <= 1:
            return s
        
        n = len(s)
        dp = [[False for _ in range(n)] for _ in range(n)]
        for i in range(n):
            dp[i][i] = True

        ans = s[0:1]
        for j in range(n):
            for i in range(i-1, -1, -1):
                if s[i] == s[j] and (j-i < 2 or dp[i+1][j-1]):
                    dp[i][j] = True
                    if j - i + 1 > len(ans):
                        ans = s[i:j+1]
        return ans
```

<p>2. Better Solution</p>

```python
class Solution:
    def longestPalindrome(s: str) -> str:
        if len(s) <= 1:
            return s
        
        i, l = 0, 0
        for j in range(len(s)):
            if s[j-l: j+1] == s[j-l: j+1][::-1]: # 홀수
                i, l = j-l, l+1
            elif j-l > 0 and s[j-l-1: j+1] == s[j-l-1: j+1][::-1]: # 짝수
                i, l = j-l-1, l+2
        return s[i: i+l]
```
