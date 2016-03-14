title: k-sum问题[leetcode]
tags: [learning, 算法, leetcode]
type: tags
date: 2016-02-28 15:28:56
---
k-sum问题：
给定一维数组，求k个数的和与给定的target值之间的关系。
如2sum, 3sum, 3sum closest, 4sum问题。

方法：
先排序，再采用双指针方法

关键：
去重的方法和步骤
<pre>
	(1) 先排序
	(2) 去除外层循序里的重复（若存在）
	(3) 和rlt结果中的上一个结果比较去重
</pre>

** 2sum问题 **
两个指针即可，left和right
复杂的度为排序的复杂的最好情况下O(nlog(n)), 除了排序，解决该问题需要的复杂的为O(n)。

** 3sum问题 **
因为3个数字，所有需要先固定一个数字，再用left和right来寻找另外两个。
复杂度为O(n*n)。

** 4sum问题 **
因为4个数字，所有需要先固定两个个数字，再用left和right来寻找另外两个。
复杂度为O(n * n * n)。
<!-- more -->

### ** 2sum问题代码如下： **
问题：
Given an array of integers, return indices of the two numbers such that they add up to a specific target.
You may assume that each input would have exactly one solution.

<pre>
	int cmpfunc (const void * a, const void * b)
	{
	   return ( *(int*)a - *(int*)b );
	}
	int* twoSum(int* nums, int numsSize, int target) {
	    if ( nums == NULL || numsSize < 2) return NULL;
	    int* temp = (int*)malloc( sizeof(int) * numsSize );
	    for ( int i = 0; i < numsSize; i ++ ) temp[i] = nums[i];
	    qsort( temp, numsSize, sizeof(int), cmpfunc);
	    int l = 0, h = numsSize - 1;
	    int* idx = (int*)malloc(sizeof(int)*2);
	    while( l < h ) {
	        if ( temp[l] + temp[h] == target ) break;
	        else if ( temp[l] + temp[h] > target ) h --;
	        else l ++;
	    }
	    for ( int i = 0; i < numsSize; i ++ ) if ( nums[i] == temp[l] ) idx[0] = i + 1;
	    for ( int i = 0; i < numsSize; i ++ ) if ( nums[i] == temp[h] && i != idx[0] - 1 ) idx[1] = i + 1;
	    if ( idx[0] > idx[1] ) {
	        int tmp = idx[0];
	        idx[0] = idx[1];
	        idx[1] = tmp;
	    }
	    return idx;
	}
</pre>

### ** 3sum问题 **
问题：
Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

思路：
一层循环，固定一个元素。

注意：
去重

<pre>
	int cmpfunc (const void * a, const void * b){
	   return ( *(int*)a - *(int*)b );
	}
	int** threeSum(int* nums, int numsSize, int* returnSize) {
	    int left, right;
	    int row = 200, size = -1, total = 0;
	    int sum = 0;
	    int** rlt = (int **)malloc(sizeof(int *) * row); 
	    memset(rlt, 0, sizeof(rlt));
	    if (numsSize < 3) return  rlt;
	    
	    qsort( nums, numsSize, sizeof(int), cmpfunc);
	    
	    for (int i = 0; i < numsSize; i++) {
	        if(i > 0 && nums[i] == nums[i-1]){  
	            continue;  
	        } 
	        
	        left = i+1;
	        right = numsSize - 1;
	        while (left < right) {
	            sum = nums[i] + nums[left] + nums[right];
	            if (sum == 0) {
	               if (size == -1 || size >= 0 && (rlt[size][0] != nums[i] || rlt[size][1] != nums[left] ||
	                   rlt[size][2] != nums[right])) {
	                   rlt[++size] = (int *)malloc(sizeof(int)* 3);
	                   
	                   rlt[size][0] = nums[i];
	                   rlt[size][1] = nums[left];
	                   rlt[size][2] = nums[right];
	               }
	               left++;right--;
	            } else if (sum > 0){
	               right--;
	            } else {
	               left++;
	            }
	        }
	    }
	    *returnSize = size + 1;
	    
	    return rlt;
	}
</pre>

### ** 3-sum-closest问题 **
问题：
Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

<pre>
	int cmpfunc (const void * a, const void * b){
	   return ( *(int*)a - *(int*)b );
	}

	int threeSumClosest(int* nums, int numsSize, int target) {
	    int left, right;
	    int row = 100, size = -1, total = 0;
	    int sum = 0;
	    if (numsSize < 3) return  0;
	    
	    qsort( nums, numsSize, sizeof(int), cmpfunc);
	    
	    int closest = nums[0] + nums[1] + nums[2];
	    
	    for (int i = 0; i < numsSize; i++) {
	        left = i+1;
	        right = numsSize - 1;
	        while (left < right) {
	            sum = nums[i] + nums[left] + nums[right];
	            int preDelta  = abs(sum - target);
	            int postDelta = abs(closest - target);
	            
	            if (preDelta == postDelta) {
	               left++;
	               right--;
	            } else{
	                if ( sum - target > 0) right--;
	                else left++;
	            }
	            
	            if (preDelta == postDelta || preDelta < postDelta) {
	            	closest = sum;
	            }
	        }
	    }
	    
	    return closest;
	}
</pre>

### ** 4-sum问题 **
问题： 
Given an array S of n integers, are there elements a, b, c, and d in S such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target

思路：
两层循环，固定其中两个元素。

注意点：
注意添加第二个元素的去重条件

	int cmpfunc (const void * a, const void * b){
	   return ( *(int*)a - *(int*)b );
	}
	int** fourSum(int* nums, int numsSize, int target, int* returnSize) {
	    int left, right;
	    int row = 1000, size = -1, total = 0;
	    int sum = 0;
	    int** rlt = (int **)malloc(sizeof(int *) * row); 
	    memset(rlt, 0, sizeof(rlt));
	    if (numsSize < 4) return  rlt;
	    
	    qsort( nums, numsSize, sizeof(int), cmpfunc);
	    
	    for (int i = 0; i < numsSize - 1; i++) {
	        if(i > 0 && nums[i] == nums[i-1]){  
	            continue;  
	        } 
	        for (int j = i+1; j < numsSize; j++){ 
	            if(j > i+1 && nums[j] == nums[j-1]) continue;
	            left = j+1;
	            right = numsSize - 1;
	            
	            while (left < right) {
	                sum = nums[i] + nums[j] + nums[left] + nums[right];
	                if (sum == target) {
	                   if (size == -1 || size >= 0 && (rlt[size][0] != nums[i] || rlt[size][1] != nums[j] || rlt[size][2] != nums[left] ||
	                       rlt[size][3] != nums[right])) {
	                       rlt[++size] = (int *)malloc(sizeof(int)* 4);
	                       memset(rlt[size], 0, sizeof(rlt[size]));
	                       rlt[size][0] = nums[i];
	                       rlt[size][1] = nums[j];
	                       rlt[size][2] = nums[left];
	                       rlt[size][3] = nums[right];
	                   }
	                   left++;right--;
	                } else if (sum > target){
	                   right--;
	                } else {
	                   left++;
	                }
	            }
	        }   
	    }
	    
	    *returnSize = size + 1;
	    
	    return rlt;
	}


