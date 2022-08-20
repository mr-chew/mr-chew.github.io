---
layout: post
title:  Container With Most Water [LeetCode]
categories: [LeetCode]
---

You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).
Find two lines that together with the x-axis form a container, such that the container contains the most water.
Return the maximum amount of water a container can store.
Notice that you may not slant the container.
[link to problem on LeetCode](https://leetcode.com/problems/container-with-most-water/ "LeetCode's Homepage")

### My attempt on the problem

The further the lines are apart (width), the greater the size of the container (area) will be. The area form under the line will be limited by the shorter line (height)

I will use 3 variables to keep track of the current max area, left and right pointers. The left pointer initialized to the first elements of the list and the right pointer initialized to the last elements of the list. The current max area will be initialized to 0.

Therefore the height of the container will be the lower of the two elements at the left and right pointers and the width will be the difference between the right pointer and left pointer.

The function will compute the area between the 2 pointer and compare result with current max area. If the result is greater than current max area, it will update it as the current max area. Thereafter, it compare the height of the 2 pointer and shift the pointer that is shorter [to compensate for the reduction in width, we want to move the pointer that is shorter to a taller line]

### Explaination of the codes

I create a class named `Solution` with a function `maxArea` which take in a `list` of `integers` (height) and return an `integer`. There are 3 variables, `current_max_area` (this variable is used to store the current computed max area which is set at 0), `left` pointer (value set to the first integer position in the list) and `right` pointer (value set to the last integer position in the list). 

```python
class Solution:
    def maxArea(self, height: list[int]) -> int:
        current_max_area = 0
        left = 0
        right = len(height)-1
```

A `while` loop is used to determine the max area of the container. The `while` loop is valid as long the left pointer current position did not meet or move past the right pointer current position. The current area will be determined by the distance between the left and right pointer (width) multiply by the shorter pointer (height) 
```python
        while (left < right):
            area = (right - left) * min(height[left], height[right])

```

The computed area will be compared with the `current_max_area`, if the computed area is greater, it will be set as the `current_max_area`.
```python
            if area > current_max_area:
                current_max_area = area
```

As we reduced the width (i.e. shifting the pointer inwards), We want to shift the pointer with the shorter height inwards to compensate for the reduction in width. The height of the left and right pointer will be compared and the one that is shorter will be shifted inwards.

Once the while loop exit, the function will return the `current_max_area` which is the maximum amount of water the container can contained.  
```python
            if height[left] < height[right]:
                left += 1
            else:
                right -= 1
                
        return current_max_area
```

To run the function written above, we will assign values to integer list variable `a` and instantiate `sol` as the `Solution` class. To run the maxArea function, `sol.maxArea(a)` is used and the `print` function is used to output the result on our terminal
```python
a = [1,8,6,2,5,4,8,3,7]
sol = Solution()
print(sol.maxArea(a))
```

Here is the entire code block for reference
```python
class Solution:
    def maxArea(self, height: list[int]) -> int:
        current_max_area = 0
        left = 0
        right = len(height)-1
        
        while (left < right):
            area = (right - left) * min(height[left], height[right])
            if area > current_max_area:
                current_max_area = area
            if height[left] < height[right]:
                left += 1
            else:
                right -= 1
                
        return current_max_area

a = [1,8,6,2,5,4,8,3,7]
sol = Solution()
print(sol.maxArea(a))
```
[link to my Github repo](https://github.com/mr-chew/leetcode/blob/main/container-with-most-water.py "My Github's repo")