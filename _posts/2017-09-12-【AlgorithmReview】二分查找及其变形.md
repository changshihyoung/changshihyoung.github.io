---
layout:     post
title:      【AlgorithmReview】二分查找及其变形
subtitle:   
date:       2017-09-12
author:     ChangShihyoung
header-img: img/post-header-AlgorithmReview-background.png
catalog: true
tags:
    - AlgorithmReview
---

#### 二分查找的问题分类：  
**取整方式：**  
向下取整 向上取整（共2种）  
**区间开闭：**  
闭区间 左闭右开区间 左开右闭区间 开区间（共4种）  
**问题类型：**  
对于不下降序列a，求最小的i，使得a[i] = ke  
对于不下降序列a，求最大的i，使得a[i] = key  
对于不下降序列a，求最小的i，使得a[i] > key  
对于不下降序列a，求最大的i，使得a[i] < key  
对于不上升序列a，求最小的i，使得a[i] = key  
对于不上升序列a，求最大的i，使得a[i] = key  
对于不上升序列a，求最小的i，使得a[i] < key  
对于不上升序列a，求最大的i，使得a[i] > key  
（共8种）  
**综上所述，二分查找共有`2*4*8=64`种写法。**  
**以下选取最保险的方式进行实现。**  

#### 二分查找（**升序，无重复项**）的三个关键点：  
来自：[知乎-二分查找有几种写法-胖胖的回答](https://www.zhihu.com/question/36132386)  
- **中止条件：**`low <= high`  
`low == high` 做终结条件，会被跳过，如：[1, 5]中找0。  
只有当`low > high`的情况下，搜索条件为空，跳出。  
- **中间点选取：**`mid = low + (high - low)/2`  
搜索空间变为闭区间`[low, high]`，更加保险。  
`(low + high)/2`会使得加法溢出，不可取。  
- **返回值：** 找不到时返回`low`  
找到时：返回`mid`即可。  
找不到时：`high`的范围是`[-1, size()-1]`，`-1`会出错；`low`的范围是`[0, size()]`，可以返回。  

#### 二分查找具体实现  
详细参考：[博客-你真的会写二分查找吗](https://www.cnblogs.com/bofengyu/p/6761389.html)  
- 标准二分查找  
```C++
class Solution {
public:
    int BinarySearch(vector<int>& numbers, int target) {
		int left = 0, right = numbers.size() - 1;
		while(left <= right){
			int mid = left + (right - left)/2;
			if(numbers[mid] == target)
				return mid;
			else if(numbers[mid] < target)
				left = mid + 1;
			else
				right = mid - 1;
		}
		return left;
    }
};
```
- 查找最后一个小于key的元素  
```C++
class Solution {
public:
    int BinarySearch(vector<int>& numbers, int target) {
		int left = 0, right = numbers.size() - 1;
		while(left <= right){
			int mid = left + (right - left)/2;
			if(numbers[mid] >= target)
				right = mid - 1;
			else
				left = mid + 1;
		}
		return right;  //不存在时返回-1
    }
};
```
- 查找第一个大于等于key的元素
```C++
class Solution {
public:
    int BinarySearch(vector<int>& numbers, int target) {
		int left = 0, right = numbers.size() - 1;
		while(left <= right){
			int mid = left + (right - left)/2;
			if(numbers[mid] >= target)
				right = mid - 1;
			else
				left = mid + 1;
		}
		return left;
    }
};
```
- 查找最后一个小于等于key的元素  
```C++
class Solution {
public:
    int BinarySearch(vector<int>& numbers, int target) {
		int left = 0, right = numbers.size() - 1;
		while(left <= right){
			int mid = left + (right - left)/2;
			if(numbers[mid] <= target)
				left = mid + 1;
			else
				right = mid - 1;
		}
		return right;  //不存在时返回-1
    }
};
```
- 查找第一个大于key的元素  
```C++
class Solution {
public:
    int BinarySearch(vector<int>& numbers, int target) {
		int left = 0, right = numbers.size() - 1;
		while(left <= right){
			int mid = left + (right - left)/2;
			if(numbers[mid] <= target)
				left = mid + 1;
			else
				right = mid - 1;
		}
		return left;  //不存在时返回numbers.size()
    }
};
```
- 查找第一个与key相等的元素  
```C++
class Solution {
public:
    int BinarySearch(vector<int>& numbers, int target) {
		int left = 0, right = numbers.size() - 1;
		while(left <= right){
			int mid = left + (right - left)/2;
			if(numbers[mid] >= target)
				right = mid - 1;
			else
				left = mid + 1;
		}
		//最终情况：num[right] < key <= num[left]
		//right是最后一个小于key的
		//left是第一个大于等于key的
		if(left<numbers.size() && numbers[left]==target) //不存在时，left会超出范围
			return left;
		else
			return -1;
    }
};
```
- 查找最后一个与key相等的元素  
```C++
class Solution {
public:
    int BinarySearch(vector<int>& numbers, int target) {
		int left = 0, right = numbers.size() - 1;
		while(left <= right){
			int mid = left + (right - left)/2;
			else if(numbers[mid] <= target)
				left = mid + 1;
			else
				right = mid - 1;
		}
		//最终情况：num[right] <= key < num[left]
		//right是最后一个小于等于key的
		//left是第一个大于key的
		if(right>=0 && numbers[right]==target) //不存在时，right会超出范围
			return right;
		else
			return -1;
    }
};
```

![二分查找的变形总结](https://github.com/changshihyoung/changshihyoung.github.io/blob/master/img/post-2017-9-27-graph-1.png?raw=true)
