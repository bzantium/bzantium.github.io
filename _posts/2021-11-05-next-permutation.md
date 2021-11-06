---
layout: post
title: "[LeetCode 100] 31. Next Permutation"
subtitle: 
date: 2021-11-04
background: '/img/posts/code.jpg'
---

<h3>Problem:</h3>
<p>
Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers. If such an arrangement is not possible, it must rearrange it as the lowest possible order (i.e., sorted in ascending order). The replacement must be in place and use only constant extra memory.
</p>

```
Example 1:
Input: nums = [1,2,3]
Output: [1,3,2]

Example 2:
Input: nums = [3,2,1]
Output: [1,2,3]

Example 3:
Input: nums = [1,1,5]
Output: [1,5,1]

Example 4:
Input: nums = [1]
Output: [1]
```

<br/>
<h3>Solution:</h3>

<p>
뒤에서부터 처음으로 감소하는 숫자를 찾아내고 (nums[i-1]), 그 숫자로부터 마지막까지 돌면서 nums[i] 보다 큰 값 중에 가장 작은 숫자 (가장 뒤쪽에 있는 숫자)를 찾아 스왑한다.
그 후 i번 째 숫자부터 뒤집어서 배치하면 된다. 만약 감소하는 숫자가 없을 경우, 전체 배열을 뒤집어서 배치하면 된다.
</p>

<br/>
<h3>Code:</h3>

```python
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        desc_idx = -1
        for i in range(len(nums) - 1, 0, -1):
            if nums[i-1] < nums[i]:
                desc_idx = i - 1
                break

        if desc_idx >= 0:
            swap_idx = desc_idx
            for j in range(desc_idx + 1, len(nums)):
                if nums[desc_idx] < nums[j]:
                     swap_idx = j
            self.swap(nums, desc_idx, swap_idx)
            
        self.reverse(nums, desc_idx + 1)
        
    def swap(self, L, i, j):
        temp = L[i]
        L[i] = L[j]
        L[j] = temp
    
    def reverse(self, L, start):
        i = start
        j = len(L) - 1
        while i < j:
            self.swap(L, i, j)
            i += 1
            j -= 1
```
