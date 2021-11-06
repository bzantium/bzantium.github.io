---
layout: post
title: "[LeetCode 100] 22. Generate Parentheses"
subtitle: 
date: 2021-11-04
background: '/img/posts/code.jpg'
---

<h3>Problem:</h3>
<p>
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.
</p>

```
Example 1:
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]

Example 2:
Input: n = 1
Output: ["()"]
```

<br/>
<h3>Solution:</h3>

<p>
순열, 조합을 나열하는 문제의 경우, 먼저 순열 및 조합을 구하는 방법을 알아야 한다.
<br/>
순열: 서로 다른 n개의 원소에서 r개를 중복없이 골라 순서대로 나열하는 경우의 수
<br/>
조합: 서로 다른 n개의 원소에서 r개를 뽑는 경우의 수
</p>

```python
# 순열을 구하는 함수
def permutation(arr, r):
    arr = sorted(arr)
    used = [0 for _ in range(len(arr))]

    def generate(chosen=[], used=[]):
        if len(chosen) == r:
            print(chosen)
            return
	
        for i in range(len(arr)):
            if not used[i]:
                chosen.append(arr[i])
                used[i] = 1
                generate(chosen, used)
                used[i] = 0
                chosen.pop()
    generate([], used)
```

```python
# 조합을 구하는 함수
def combination(arr, r):
    arr = sorted(arr)
    used = [0 for _ in range(len(arr))]

    def generate(chosen=[]):
        if len(chosen) == r:
            print(chosen)
            return

        start = arr.index(chosen[-1]) + 1 if chosen else 0
        for nxt in range(start, len(arr)):
            if used[nxt] == 0 and (nxt == 0 or arr[nxt-1] != arr[nxt] or used[nxt-1]):
                chosen.append(arr[nxt])
                used[nxt] = 1
                generate(chosen)
                used[nxt] = 0
                chosen.pop()
    generate([])
```
<p>
경우의 수를 줄이기 위해서 백트래킹(Backtracking)이라는 방법을 사용할 수 있다. 백트래깅은 해를 찾아가는 도중, 지금의 경로가 해가 될 것 같지 않으면 그 경로를 더이상 가지 않고 되돌아가는 방법이다.
즉, 백트래킹은 모든 가능한 경우의 수 중에서 특정한 조건을 만족하는 경우만 살펴보는 것이다. 이 문제의 경우 특정한 조건을 만족하는 모든 수열을 구하는 방법이므로 수열을 구하는 방법과 백트래킹 알고리즘을 조합하면 풀 수 있다.
</p>

<br/>
<h3>Code:</h3>

```python
class Solution(object):
    def generateParenthesis(self, n):
        def generate(A = []):
            if len(A) == 2 * n:
                if valid(A):
                    ans.append("".join(A))
            else:
                A.append('(')
                generate(A)
                A.pop()
                A.append(')')
                generate(A)
                A.pop()

        def valid(A):
            bal = 0
            for c in A:
                if c == '(': bal += 1
                else: bal -= 1
                if bal < 0: return False
            return bal == 0

        ans = []
        generate()
        return ans
```

```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        ans = []
        def backtrack(S=[], left=0, right=0):
            if len(S) == 2 * n:
                ans.append(''.join(S))
                return
            
            if left < n:
                S.append('(')
                backtrack(S, left+1, right)
                S.pop()

            if right < left:
                S.append(')')
                backtrack(S, left, right+1)
                S.pop()
        backtrack()
        return ans
```
