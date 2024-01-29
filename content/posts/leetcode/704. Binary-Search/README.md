+++
title = '704-Binary Search'
date = 2024-01-29T16:19:34+08:00
draft = false
categories = ['LeetCode']
tags = ['Algorithm', 'Array', 'Binary Search', 'Easy']
layout = 'page'

+++

# [704. Binary Search](https://leetcode.com/problems/binary-search/description/)

## 题目描述

Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.



Example & Constraints:

```go
Example 1:

Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4
Example 2:

Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
 

Constraints:

1 <= nums.length <= 104
-104 < nums[i], target < 104
All the integers in nums are unique.
nums is sorted in ascending order.
```



## 题目大意

从元素各不相同的升序数组中找到目标元素，返回对应的索引位置，如果元素不存在则返回 -1



## 题目解决思路

当数组元素是有序的时候，一般都会考虑到二分搜索找到目标元素，不过这道题有几种变体，分别是：

1. 数组是非降序(或非升序)，找到数组中元素等于目标值的数组最左侧索引值
2. 数组是非降序(或非升序)，找到数组中元素等于目标值的数组最右侧索引值
3. 数组是非降序(或非升序)，找到数组中元素等于目标值的数组最左侧索引值及最右侧索引值的数组

上面列举的第三种情况实际上是第一、第二种情况的组合，结合实际情况做相关判断处理(如最左侧索引值和最右侧索引值相同如何处理)



## Go 核心代码实现

```go
// 左闭右开
func search(nums []int, target int) int {
	left, right := 0, len(nums)
	for left < right {
		mid := left + (right-left)>>1
		if nums[mid] == target {
			return mid
		} else if nums[mid] < target {
			left = mid + 1
		} else {
			right = mid
		}
	}
	return -1
}

// 左闭右闭
func searchV2(nums []int, target int) int {
	left, right := 0, len(nums) - 1
	for left <= right {
		mid := left + (right - left) >> 1
		if nums[mid] == target {
			return mid 
		} else if nums[mid] < target {
			left = mid + 1
		} else {
			right = mid - 1
		}
	}
	return -1
}

```



变体1: 找到数组中元素等于目标值的数组最左侧索引值

```
func searchMinLeft(nums []int, target int) int {
	left, right := 0, len(nums)
	var index int = -1
	for left < right {
		mid := left + (right-left)>>1
		if nums[mid] == target {
			index = mid
			right = mid
		} else if nums[mid] < target {
			left = mid + 1
		} else {
			right = mid
		}
	}
	return index
}
```



变体2: 找到数组中元素等于目标值的数组最右侧索引值

```go
func searchMaxRight(nums []int, target int) int {
	left, right := 0, len(nums)
	var index int = -1
	for left < right {
		mid := left + (right-left)>>1
		if nums[mid] == target {
			index = mid
			left = mid + 1
		} else if nums[mid] < target {
			left = mid + 1
		} else {
			right = mid
		}
	}
	return index
}
```



测试用例完整代码：

```go
package leetcode

import (
	"math/rand"
	"sort"
	"testing"
	"time"
)

const (
	minArrLen = 1
	maxArrLen = 104
	maxArrVal = 104
)

func searchBruce(nums []int, target int) int {
	var result int = -1
	for index, num := range nums {
		if num == target {
			result = index
			// add break or not
			// break
		}
	}
	return result
}

func search(nums []int, target int) int {
	left, right := 0, len(nums)
	for left < right {
		mid := left + (right-left)>>1
		if nums[mid] == target {
			return mid
		} else if nums[mid] < target {
			left = mid + 1
		} else {
			right = mid
		}
	}
	return -1
}

func generateArrLen(min, max int) int {
	r := rand.New(rand.NewSource(time.Now().UnixNano()))
	return r.Intn(max-min+1) + min
}

// arr val (-max, max)   [0, 2max + 1) - (max + 1)
func generateArrVal(max int) int {
	r := rand.New(rand.NewSource(time.Now().UnixNano()))
	return r.Intn(2*max+1) - (max + 1)
}

// unique controll wheather array element could be unique or not
func generateArray(max int, unique bool) []int {
	arrLen := generateArrLen(minArrLen, maxArrLen)
	arr := make([]int, arrLen)
	set := make(map[int]struct{})

	for i := 0; i < arrLen; i++ {
		if unique {
			val := generateArrVal(max)
			for {
				if _, ok := set[val]; !ok {
					set[val] = struct{}{}
					arr[i] = val
					break
				}
				val = generateArrVal(max)
			}
		} else {
			arr[i] = generateArrVal(max)
		}
	}

	sort.Ints(arr)
	return arr
}

// 这里也可以替换成其它测试函数
func TestBinarySearch(t *testing.T) {
	for i := 0; i < 10000; i++ {
		arr := generateArray(maxArrVal, true)
		targetVal := generateArrVal(maxArrVal)
		got := search(arr, targetVal)
		want := searchBruce(arr, targetVal)
		if got != want {
			t.Fatalf("arr: %v\n target val: %v\n got: %v want: %v", arr, targetVal, got, want)
		}
	}
}
```

