---
title: 【LeetCode】912. Sort an Array 解题记录
categories:
  - 科研学习 - LeetCode
tags:
  - LeetCode
  - Medium
  - 排序
  - 算法
cover: /images/cover/leetcode.jpeg
abbrlink: '6854315'
date: 2021-07-03 17:36:36
---


# 问题描述

Given an array of integers nums, sort the array in ascending order.

## 测试样例

```
Input: nums = [5,2,3,1]
Output: [1,2,3,5]]
```

```
Input: nums = [5,1,1,2,0,0]
Output: [0,0,1,1,2,5]
```

## 说明

```
1 <= nums.length <= 5 * 10^4
-5 * 10^4 <= nums[i] <= 5 * 10^4
```

# 解题

## 思路

1. 插入排序 O(n^2)
2. 冒泡排序 O(n^2)
3. 选择排序 O(n^2)
4. 快排 O(nlogn)
5. 归并排序 O(nlogn)

**补充：**

1. 插入/冒泡/选择排序复杂度较高，会超时，这里仅实现

## 代码

```java
class Solution {
    public int[] sortArray(int[] nums) {
        
        // return insertionSort(nums);              // 插入排序
        // return bubbleSort(nums);                 // 冒泡排序
        // return selectionSort(nums);              // 选择排序
        // quickSort(nums, 0, nums.length - 1);     // 快速排序
        
        int[] temp = new int[nums.length];
        mergeSort(nums, 0, nums.length, temp);      // 归并排序
        return nums;
    }
    
    /*
     * 插入排序
     */
    public int[] insertionSort(int[] nums) {
        // 逐个选取
        for(int i=0; i<nums.length; ++i) {
            // 不断前移待排序元素，直到找到插入位置
            for(int j=i; j>0 && nums[j-1] > nums[j]; --j) {
                // 前移待排序元素
                swap(nums, j, j-1);
            }
        }
        return nums;
    }
    /*
     * 冒泡排序
     */
    public int[] bubbleSort(int[] nums) {
        // 第 i 次冒泡
        for(int i=1; i<nums.length; ++i) {
            boolean swapped = false;        // 本次冒泡是否交换过元素
            for(int j=1; j<nums.length-i+1; ++j) {  // 从第 1 个元素开始，数组末尾 i 个元素已排好
                if(nums[j] < nums[j-1]) {               // 若相邻元素顺序错误
                    swap(nums, j, j-1);                     // 交换
                    swapped = true;
                }
            }
            if(!swapped) break;     // 若某次冒泡未交换，则已完成排序
        }
        return nums;
    }
    
    /*
     * 选择排序
     */
    public int[] selectionSort(int[] nums) {
        // 选择一个位置
        for(int i=0; i<nums.length; ++i) {
            int idx_sorting = i;    // 当前位置正确元素的坐标
            for(int j=i+1; j<nums.length; ++j) {    // 与后面的元素比较
                if(nums[idx_sorting] > nums[j]) {       // 选择最小的元素
                    idx_sorting = j;
                }
            }
            swap(nums, i, idx_sorting);     // 将正确元素置于当前位置
        }
        return nums;
    }
    
    /*
     * 快排
     */
    public void quickSort(int[] nums, int l, int r) {
        if(l >= r) {    // 已处理完
            return;
        }
        
        int beg = l, end = r;   // 左右指针
        int elem_sorting = nums[l];     // 待排序元素，基准
        
        while(beg < end) {
            while(beg < end && nums[end] >= elem_sorting) {     // 在右侧寻找比基准小的元素
                --end;
            }
            nums[beg] = nums[end];      // 将右侧小的元素换到左侧
            while(beg < end && nums[beg] < elem_sorting) {      // 在左侧寻找比基准大的元素
                ++beg;
            }
            nums[end] = nums[beg];      // 将左侧大的元素换到右侧
        }
        nums[beg] = elem_sorting;   // 将基准置于正确的位置
        // 递归基准左侧
        quickSort(nums, l, beg - 1);
        // 递归基准右侧
        quickSort(nums, beg + 1, r);
    }
    
    /*
     * 归并排序
     */
    public void mergeSort(int[] nums, int l, int r, int[] temp) {
        if(l + 1 >= r) {    // 已处理完
            return;
        }
        
        int mid = (l + r) / 2;
        // 分割
        mergeSort(nums, l, mid, temp);
        mergeSort(nums, mid, r, temp);
        
        // 左侧指针，右侧指针，待确定元素位置
        int p1 = l, p2 = mid, idx = l;
        // 合并
        while(p1 < mid || p2 < r) {     // 遍历两侧
            
            if(p2 >= r || (p1 < mid && nums[p1] <= nums[p2])) {      // 右侧已遍历完或左侧更小
                temp[idx++] = nums[p1++];
            } else {                                                // 左侧已遍历完或右侧更小
                temp[idx++] = nums[p2++];
            }
        }
        // 复制已排序号的数组
        for(int i=l; i<r; ++i) {
            nums[i] = temp[i];
        }
    }
    
    /*
     * 交换数组中两元素的位置
     */
    public void swap(int[] arrays, int idx_a, int idx_b) {
        int elem = arrays[idx_a];
        arrays[idx_a] = arrays[idx_b];
        arrays[idx_b] = elem;
    }
}
```


