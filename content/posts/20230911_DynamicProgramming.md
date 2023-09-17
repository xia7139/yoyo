+++
title = "动态规划算法"
author = ["Cheng Xia"]
draft = false
+++

<div class="ox-hugo-toc toc">

<div class="heading">Table of Contents</div>

- [介绍](#介绍)
- [示例](#示例)
    - [最长递增子序列](#最长递增子序列)
        - [问题定义](#问题定义)
        - [问题解决](#问题解决)
        - [参考代码](#参考代码)
    - [青蛙跳阶问题](#青蛙跳阶问题)
        - [问题定义](#问题定义)
        - [问题解决](#问题解决)
        - [参考代码](#参考代码)

</div>
<!--endtoc-->



## 介绍 {#介绍}

Dynamic programming is a method for solving a complex problem by breaking it down into a collection of simpler subproblems. <br/>

动态规划（英语：Dynamic programming，简称 DP），是一种在数学、管理科学、计算机科学、经济学和生物信息学中使用的，通过把原问题分解为相对简单的子问题的方式求解复杂问题的方法。动态规划常常适用于有重叠子问题和最优子结构性质的问题。 <br/>

简单的说，动态规划，就是将问题拆分成更简单的子问题，通过求解子问题，记录子问题的解，逐步求解到问题本身的过程。至于如何构造子问题，以及如何构造子问题和问题的联系，就全靠水平和悟性了。[^fn:1] <br/>


## 示例 {#示例}


### 最长递增子序列 {#最长递增子序列}


#### 问题定义 {#问题定义}

输入一个无序的整数数组，请你找到其中最长的严格递增子序列的长度，函数签名如下： <br/>

int lengthOfLIS(int[] nums); <br/>
比如说输入 nums=[10,9,2,5,3,7,101,18]，其中最长的递增子序列是 [2,3,7,101]，所以算法的输出应该是 4。 <br/>

注意「子序列」和「子串」这两个名词的区别，子串一定是连续的，而子序列不一定是连续的。 <br/>


#### 问题解决 {#问题解决}

这里比较巧妙的一点在于，构件一个子问题：以第i个元素结尾的最大子序列长度。 <br/>

在求解第i个元素结尾最长子序列时，只需要逐一对前面的i-1个元素进行判断，比如，现在是第x(x &lt;= i-1)个元素：如果第i个元素大于第x个元素，那么将第i个元素放在第x元素的后面可以组成一个递增子序列，其长度就是以第x个元素结尾的最长子序列的长度+1；如果第i个元素小于等于第x个元素，那么，这时候，在这个判断中，就认为子序列是第i个元素本身，其长度是1。 <br/>

前面的i-1个元素都判断完成后，取子序列长度的最大值，就认为是以第i个元素结尾的最大子序列长度。 <br/>

在求解数组中的每个元素结尾的最长递增子序列长度的过程中，依次记下最大值，就是整个数组的最长子序列的长度。 <br/>


#### 参考代码 {#参考代码}

见src/com/lfqy/trying/dynamicprogramming/LongestIncrementalSequence.java： <br/>

```java
package com.lfqy.trying.dynamicprogramming;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;

public class LongestIncrementalSequence {
    /**
     * 求数组最长递增子序列的长度
     * */
    public static int getLengthOfLIS(int[] nums){
	int lengthOfLIS = 0;
	//如果数组长度为0，最长递增子序列长度为0
	if(nums.length == 0){
	    lengthOfLIS = 0;
	}else{
	    //非空数组的最长递增子序列长度至少为1
	    lengthOfLIS = 1;
	    //建一个同长度的数组，数组的第i个元素，存储nums数组中以第i个元素结尾的最长递增子序列长度
	    int []lengthOfLISEndAtI = new int[nums.length];
	    //以第0个元素结尾的最长递增子序列长度为1
	    lengthOfLISEndAtI[0] = 1;
	    for(int i = 1; i < nums.length; i++){
		int currLengthOfIS = 1;
		for(int j = 0; j < i; j++){
		    if(nums[j] < nums[i]){
			currLengthOfIS = Math.max(lengthOfLISEndAtI[j] + 1, currLengthOfIS);
		    }
		}
		lengthOfLISEndAtI[i] = currLengthOfIS;
		lengthOfLIS = Math.max(lengthOfLIS, currLengthOfIS);
	    }
	    System.out.println("Length of LIS end at i: " + Arrays.toString(lengthOfLISEndAtI));
	}
	return lengthOfLIS;
    }
    /**
     * 打印前面函数中nums数组中每个元素结尾的最长递增子序列，也就是lengthOfLISEndAtI[i]对应的最长递增子序列
     * */
    public static void printLISAtI(int[] nums){
	int lengthOfLIS = 0;
	//如果数组长度为0，最长递增子序列长度为0
	if(nums.length == 0){
	    lengthOfLIS = 0;
	}else{
	    //非空数组的最长递增子序列长度至少为1
	    lengthOfLIS = 1;
	    //建一个同长度的数组，数组的第i个元素，存储nums数组中以第i个元素结尾的最长递增子序列长度
	    int []lengthOfLISEndAtI = new int[nums.length];
	    int []priorPosOfLISEndAtI = new int[nums.length];
	    //以第0个元素结尾的最长递增子序列长度为1
	    lengthOfLISEndAtI[0] = 1;
	    priorPosOfLISEndAtI[0] = -1;
	    for(int i = 1; i < nums.length; i++){
		int currLengthOfIS = 1;
		priorPosOfLISEndAtI[i] = -1;
		for(int j = 0; j < i; j++){
		    if(nums[j] < nums[i]){
			currLengthOfIS = Math.max(lengthOfLISEndAtI[j] + 1, currLengthOfIS);
			if(currLengthOfIS == (lengthOfLISEndAtI[j] + 1)){
			    priorPosOfLISEndAtI[i] = j;
			}
		    }
		}
		lengthOfLISEndAtI[i] = currLengthOfIS;
		lengthOfLIS = Math.max(lengthOfLIS, currLengthOfIS);
	    }
	    System.out.println("Prior position of LIS end at i: " + Arrays.toString(priorPosOfLISEndAtI));
	    for(int i = 0; i < nums.length; i++){
		System.out.println("LIS end with [" + i + "]:");
		List listLIS = new ArrayList<Integer>();
		for(int j = i; j >= 0;){
		    listLIS.add(nums[j]);
		    if(priorPosOfLISEndAtI[j] == -1){
			break;
		    }else{
			j = priorPosOfLISEndAtI[j];
		    }
		}
		//System.out.println("listLIS: " + listLIS);
		Collections.reverse(listLIS);
		System.out.println("listLIS revsersed: " + listLIS);
		System.out.println("");
	    }
	}
    }
    public static void main(String []args){
	int []tmp = {10,9,2,5,3,7,101,18,3};
	System.out.println(Arrays.toString(tmp) + ", Length of Max Incremental Sequence: " + getLengthOfLIS(tmp));
	printLISAtI(tmp);
    }
}
```

com.lfqy.trying.dynamicprogramming.LongestIncrementalSequence#getLengthOfLIS方法中，展示了前面说的求解最长子序列长度的过程。com.lfqy.trying.dynamicprogramming.LongestIncrementalSequence#printLISAtI方法中，除了求出了最长递增子序列的长度，也打印出了求解过程中，以每个元素结尾的最长子序列是什么。 <br/>


### 青蛙跳阶问题 {#青蛙跳阶问题}


#### 问题定义 {#问题定义}

一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个n级的台阶总共有多少种跳法。 <br/>


#### 问题解决 {#问题解决}

这个问题，比较直观的思路，就是倒着考虑。青蛙最后一跳，有两种可能，跳了一级台阶，或者跳了两级台阶。这样，如果记f(n)为跳上n级台阶有多少种跳法，可以得出：f(n)=f(n-1)+f(n-2)。 <br/>

这样，再反过来，从小往大去考虑，很容易知道，f(1)=1，f(2)=2，往下就可以求f(3)=f(2)+f(1)。再往下，就可以用f(2)和f(3)求f(4)，依次类推，可以得到只需要保留最前面的两项，就可以一直这样往前求值下去。 <br/>


#### 参考代码 {#参考代码}

src/com/lfqy/trying/dynamicprogramming/FrogStepCompute.java，如下： <br/>

```java
package com.lfqy.trying.dynamicprogramming;

public class FrogStepCompute {
    /**
     * 一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个n级的台阶总共有多少种跳法。
     * */
    public static int getStepNum(int n){
	if(n == 0){
	    return 0;
	}
	if(n == 1){
	    return 1;
	}
	if(n == 2){
	    return 2;
	}

	int num_i = 0;
	if(n > 2){
	    int num_i_2 = 1;//i-2
	    int num_i_1 = 2;//i-1
	    for(int i = 3; i <= n; i++){
		num_i = num_i_1 + num_i_2;
		num_i_2 = num_i_1;
		num_i_1 = num_i;
	    }
	}
	return num_i;
    }

    public static void main(String []args){
	for(int i = 1; i <= 10; i++)
	    System.out.println(getStepNum(i));
    }
}
```

其中，在com.lfqy.trying.dynamicprogramming.FrogStepCompute#getStepNum方法中，实现了根据相邻的两项，再往前求下一项的逻辑。循环往复，可以求到第n项。 <br/>

[^fn:1]: [看一遍就理解：动态规划详解](https://zhuanlan.zhihu.com/p/365698607)  <br/>