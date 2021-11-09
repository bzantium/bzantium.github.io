---
layout: post
title: "[LeetCode 100] 34. Find First and Last Position of Element in Sorted Array"
subtitle: 
date: 2021-11-09
background: '/img/posts/code.jpg'
---

<h3>Problem:</h3>
<p>
Given an array of integers nums sorted in non-decreasing order, find the starting and ending position of a given target value.
If target is not found in the array, return [-1, -1].
You must write an algorithm with $O(log(n))$ runtime complexity.
</p>

```
Example 1:
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]

Example 2:
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]

Example 3:
Input: nums = [], target = 0
Output: [-1,-1]
```

<br/>
<h3>Solution:</h3>

<p>
시간복잡도를 $O(log(n))$을 충족시키기 위해서는 binary search 방법을 사용해야 한다.
왼쪽과 오른쪽 각각 binary search를 통해 찾아내야 한다. 숫자가 존재하지 않는다면 [-1, -1]를 리턴한다.
</p>

<br/>
<h3>Code:</h3>

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        ans = [-1, -1]
        if not nums:
            return ans
        
        low = 0
        high = len(nums) - 1

        while low < high:
            mid = (low + high) // 2
            if nums[mid] < target:
                low = mid + 1
            else:
                high = mid
        
        if target != nums[low]:
            return ans
        else:
            ans[0] = low

        high = len(nums) - 1
        
        while low < high:
            mid = (low + high) // 2 + 1
            if nums[mid] > target:
                high = mid - 1
            else:
                low = mid
        
        ans[1] = high
        return ans
```
