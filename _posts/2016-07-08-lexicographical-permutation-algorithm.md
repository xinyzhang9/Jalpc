---
layout: post
title:  "Lexicographical permutation algorithm"
date:   2016-07-08
desc: "Lexicographical permutation algorithm"
keywords: "python, algorthm, permutation"
categories: [Python,Algorithms]
tags: [python, algorthm, permutation]
icon: icon-python
---
## Problem
Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.  

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).  

The replacement must be in-place, do not allocate extra memory.  

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.  

```
1,2,3 → 1,3,2  
3,2,1 → 1,2,3  
1,1,5 → 1,5,1  
```

## Solution
For the detailed algorithm and demonstration, please go to [this article](https://www.nayuki.io/page/next-lexicographical-permutation-algorithm)  

The steps are shown in the picture below.
![alt text](https://www.nayuki.io/res/next-lexicographical-permutation-algorithm/next-permutation-algorithm.png)

```
class Solution(object):
    def nextPermutation(self, nums):
        """
        :type nums: List[int]
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        # find longest non-increasing suffix
        right = len(nums)-1
        while nums[right] <= nums[right-1] and right-1 >=0:
            right -= 1
        if right == 0:
            return self.reverse(nums,0,len(nums)-1)
        # find pivot
        pivot = right-1
        successor = 0
        # find rightmost succesor
        for i in range(len(nums)-1,pivot,-1):
            if nums[i] > nums[pivot]:
                successor = i
                break
        # swap pivot and successor
        nums[pivot],nums[successor] = nums[successor],nums[pivot]  
        # reverse suffix
        self.reverse(nums,pivot+1,len(nums)-1)
        
    def reverse(self,nums,l,r):
        while l < r:
            nums[l],nums[r] = nums[r],nums[l]
            l += 1
            r -= 1
```
## Link to Leetcode discuss board
[link](https://discuss.leetcode.com/topic/52275/easy-python-solution-based-on-lexicographical-permutation-algorithm)
