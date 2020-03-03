---
title: LeetCode 11
top: false
cover: false
toc: true
mathjax: true
date: 2020-02-12 22:08:52
tags:
password:
summary:
categories: ['LeetCode']
---
## 题目描述
给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器，且 n 的值至少为 2。
![图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)
示例:

> 输入: [1,8,6,2,5,4,8,3,7]
> 输出: 49

## 代码

1. **暴力枚举法**

   时间复杂度：$O(n^2)$

   ```javascript
   /**
    * 暴力法
    * 利用两层遍历，计算任意两个柱子所形成的容器的容量，保存最大的容量即可。
    * @param {number[]} height
    * @return {number}
    */
   var maxArea = function(height) {
       var res = 0;
       for (var i = 0; i < height.length - 1; i++) {
           for (var j = i + 1; j < height.length; j++) {
               var temp = (j - i) * Math.min(height[i],height[j]);
               if (temp > res) {
                   res = temp;
               }
           }
       }
       return res;
   };
   ```

2. **双指针法**

   - 双指针法最重要的问题就是应该移动那个指针。
   - 对该题而言，先引入一个计算容量的表达式`(right - left) * Math.min(height[left], height[right])`。
   - 因为两个指针是逐渐逼近的，所以表达式right - left肯定是渐渐变小的。
   - 但是我们所求的是最大的容量，所以在指针移动过程中，`Math.min(height[left], height[right])`必须变大才有可能得到更大的容量，从这里也就可以知道双指针在移动时应该移动`height[left]`和`height[right]`中数值小的那个。
   - 时间复杂度：$O(n)$

   ```javascript
   /**
    * 双指针法
    * @param {number[]} height
    * @return {number}
    */
   var maxArea = function(height) {
       let i = 0, j = height.length-1;
       let square, max = 0;
       while(j-i >= 1){
           if(height[i]>height[j]){
               square = height[j] * (j-i);
               j--;
           }else{
               square = height[i] * (j-i);
               i++;
           }
           max = Math.max(square,max);
       }
       return max;
   };
   ```

   

