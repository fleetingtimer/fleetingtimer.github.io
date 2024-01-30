+++
title = '27-Remove-Element'
date = 2024-01-30T22:16:05+08:00
draft = false
categories = ['LeetCode']
tags = ['Algorithm', 'Array', 'Easy']
layout = 'page'

+++

# [[27. Remove Element](https://leetcode.com/problems/remove-element/)]

## 题目描述

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm). The order of the elements may be changed. Then return *the number of elements in* `nums` *which are not equal to* `val`.

Consider the number of elements in `nums` which are not equal to `val` be `k`, to get accepted, you need to do the following things:

- Change the array `nums` such that the first `k` elements of `nums` contain the elements which are not equal to `val`. The remaining elements of `nums` are not important as well as the size of `nums`.
- Return `k`.

**Custom Judge:**

The judge will test your solution with the following code:

```
int[] nums = [...]; // Input array
int val = ...; // Value to remove
int[] expectedNums = [...]; // The expected answer with correct length.
                            // It is sorted with no values equaling val.

int k = removeElement(nums, val); // Calls your implementation

assert k == expectedNums.length;
sort(nums, 0, k); // Sort the first k elements of nums
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```

If all assertions pass, then your solution will be **accepted**.



Example & Constraints:

```go
Example 1:

Input: nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2,_,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 2.
It does not matter what you leave beyond the returned k (hence they are underscores).
Example 2:

Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
Note that the five elements can be returned in any order.
It does not matter what you leave beyond the returned k (hence they are underscores).
 

Constraints:

0 <= nums.length <= 100
0 <= nums[i] <= 50
0 <= val <= 100
```



## 题目大意

移除数组中元素值等于目标元素，并保证数组的前k个的数值不等于目标元素且出现的顺序与之前前后顺序相同，返回这个数组长度k



## 题目解决思路

设置初始指针位置为数组索引0位置，遍历整个数组，当数组元素不为目标元素将当前遍历的数组元素赋值给指针索引位置的数组元素同时指针索引向后移动，最后返回这个指针索引即可。



## Go 核心代码实现

```go
func removeElement(nums []int, val int) int {
    index := 0
    for i := 0; i < len(nums); i++ {
        if nums[i] != val {
            nums[index] = nums[i]
            index++
        }
    }
    return index
}
```
