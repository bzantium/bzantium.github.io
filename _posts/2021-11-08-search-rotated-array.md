---
layout: post
title: "[LeetCode 100] 33. Search in Rotated Sorted Array"
subtitle: 
date: 2021-11-08
background: '/img/posts/code.jpg'
---

<h3>Problem:</h3>
<p>
There is an integer array nums sorted in ascending order (with distinct values).
Prior to being passed to your function, nums is possibly rotated at an unknown pivot index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].
Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.
You must write an algorithm with $O(log(n))$ runtime complexity.
</p>

```
Example 1:
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4

Example 2:
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1

Example 3:
Input: nums = [1], target = 0
Output: -1
```

<br/>
<h3>Solution:</h3>

<p>
시간복잡도를 $O(log(n))$을 충족시키기 위해서는 binary search 방법을 사용해야 한다.
<ol>
<li> low, high = 0, len(nums) - 1 로 초기화하고 low <= high 를 충족하는 한 루프를 진행한다. </li>
<li> target == nums[mid] 일 경우, mid를 리턴한다. </li>
<li> nums[low] <= nums[mid] 일 경우, 왼쪽 구간에 target이 존재하는 지 검사한다. (nums[low] <= target <= nums[mid]) </li>
<li> 존재할 경우, high = mid - 1, 아닐경우, low = mid + 1 값으로 변경한다. </li>
<li> nums[low] > nums[mid] 일 경우, 오른쪽 구간에 target이 존재하는 지 검사한다. (nums[mid] <= target <= nums[high]) </li>
<li> 존재할 경우, low = mid + 1, 아닐경우, high = mid - 1 값으로 변경한다. </li>
<li> 루프를 벗어날 경우, -1을 리턴한다. </li>
</ol>
</p>

<br/>
<h3>Code:</h3>

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:     
        low, high = 0, len(nums) - 1

        while low <= high:
            mid = (low + high) // 2
            if target == nums[mid]:
                return mid
            if nums[low] <= nums[mid]:
                if nums[low] <= target <= nums[mid]:
                    high = mid - 1
                else:
                    low = mid + 1
            else:
                if nums[mid] <= target <= nums[high]:
                    low = mid + 1
                else:
                    high = mid - 1
        return -1
```
