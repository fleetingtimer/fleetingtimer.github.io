+++
title = '01-Two Sum'
date = 2024-01-24T19:27:42+08:00
draft = false
categories = ['LeetCode']
tags = ['Algorithm', 'Array', 'HashMap', 'Easy']
layout = 'page'

+++

# [1.Two-Sum](https://leetcode.com/problems/two-sum/)

## 题目描述

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.



Example & Constraints:
```
Example 1:

Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
Example 2:

Input: nums = [3,2,4], target = 6
Output: [1,2]
Example 3:

// 这个范例应该是错误的，出现了相同的元素
Input: nums = [3,3], target = 6
Output: [0,1]
 

Constraints:

2 <= nums.length <= 104
-109 <= nums[i] <= 109
-109 <= target <= 109
Only one valid answer exists.

Follow-up: Can you come up with an algorithm that is less than O(n2) time complexity?
```



## 题目大意

从数组元素各不相同的一维数组中找出两个数组元素的值之和为某个目标值，并返回这两个数组元素的值对应的索引位置，返回结果不区分顺序






##  题目解决思路

首先数组是我们要遍历的数据结构，在遍历数组每一个元素时如何快速定位另外一个目标元素是否存在呢？首先考虑的应该是使用哈希表，如何定义这个哈希表呢？题目已经说明数组元素互不相同，可以在遍历的时候以数组元素的值为key，以数组元素的索引为value 来创建这个哈希表。具体实现有两种，两种时间复杂度都是O(N), 第一种是基于数组生成完整的哈希表，然后再遍历数组求解。第二种是只遍历一次数组，边遍历数组元素边构建哈希表并求解。 





## GO 核心代码实现

```go
package leetcode

func twoSum(nums []int, target int) []int {
    m := make(map[int]int)
    for index, num := range nums {
        if i, ok := m[target-num]; ok {
            return []int{i, index}
        }
        m[num] = index
    }
    return nil
}
```

测试用例完整代码:

```go
package leetcode

import (
	"math/rand"
	"testing"
	"time"
)

const (
	maxVal       = 109
	maxTargetVal = 109
	minArrLen    = 2
	maxArrLen    = 104
)

func twoSum(nums []int, target int) []int {
	m := make(map[int]int)
	for index, num := range nums {
		if i, ok := m[target-num]; ok {
			return []int{i, index}
		}
		m[num] = index
	}
	return nil
}

func twoSumBruce(nums []int, target int) []int {
	for i := 0; i < len(nums)-1; i++ {
		for j := i + 1; j < len(nums); j++ {
			if nums[i]+nums[j] == target {
				return []int{i, j}
			}
		}
	}
	return nil
}

func generateNum(max int) int {
	r := rand.New(rand.NewSource(time.Now().UnixNano()))
	return r.Intn(max<<1+1) - max
}

func generateArray(target int, max int) []int {
	arrLen := rand.Intn(maxArrLen-minArrLen+1) + minArrLen
	arr := make([]int, arrLen)

	var num1, num2 int
	set := make(map[int]struct{})
	num1 = generateNum(max)
	num2 = generateNum(max)
	set[num1] = struct{}{}

	for num2 == num1 {
		num2 = generateNum(max)
	}

	set[num2] = struct{}{}
	arr[0] = num1
	arr[1] = num2

	for i := 2; i < arrLen; i++ {
		randNum := generateNum(max)
		for {
			// 元素没有重复且和已有的元素相加不能为target值
			if _, ok := set[randNum]; !ok {
				if _, ok := set[target-randNum]; !ok {
					break
				}
			}
			randNum = generateNum(max)
		}
		set[randNum] = struct{}{}
		arr[i] = randNum
	}

	r := rand.New(rand.NewSource(time.Now().UnixNano()))
	r.Shuffle(len(arr), func(i, j int) {
		arr[i], arr[j] = arr[j], arr[i]
	})
	return arr
}

func isEqual(arr1 []int, arr2 []int) bool {
	if len(arr1) != len(arr2) {
		return false
	}

	for i, v := range arr1 {
		if arr2[i] != v {
			return false
		}
	}
	return true
}

func TestTwoSum(t *testing.T) {
	arr := generateArray(maxTargetVal, maxVal)
	for i := 0; i < 100000; i++ {
		expectedRes := twoSumBruce(arr, maxTargetVal)
		res := twoSum(arr, maxTargetVal)
		if !isEqual(expectedRes, res) {
			t.Fatalf("input arr is :%v\n expect result is :%v but got %v\n", arr, expectedRes, res)
		}
	}
	t.Log("Test TwoSum Success!\n")
}

```

