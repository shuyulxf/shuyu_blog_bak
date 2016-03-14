title: bit-operation[leetcode]
tags: [learning, 算法, leetcode]
type: tags
date: 2016-02-28 21:03:34
---
### ** Divide Two Integers **
问题:
Divide two integers without using multiplication, division and mod operator.
If it is overflow, return MAX_INT.

思路：
根据每一个十进制数都可以表示成二进制数的形式，结合位移运算和减法运算，得到结果。
二进制形式为：(x为被除数，y为除数)
x = a<sub>n</sub>*2<sup>n</sup> + a<sub>n-1</sub>*2<sup>n-1</sup> + ... + a<sub>0</sub>*2 + a<sub>0</sub>
y = b<sub>n</sub>*2<sup>n</sup> + b<sub>n-1</sub>*2<sup>n-1</sup> + ... + b<sub>0</sub>*2 + b<sub>0</sub>
从最高位开始计算 | 从最低位开始计算 两个方向
<!-- more -->
注意：
边界处理
因为INT_MIN = -2147483648, INT_MAX = 2147483647
因为正负最大整数的绝对值并不相等，所以会出现一些特殊值需要处理。
<table><thead><th>dividend</th><th>divisor</th></thead><tbody><tr><td>INT_MIN</td><td>-1</td></tr><tr><td>INT_MAX</td><td>-1</td><tr><td>INT_MIN</td><td>1</td></tr></tbody></table>

以下是源代码
(1) 从最高位开始计算

	int divide(int dividend, int divisor) {

		long long a = dividend;
		long long b = divisor;
		char sign = 1;
		
	    if (dividend <= INT_MIN && divisor == -1) return INT_MAX;
	    if (dividend >= INT_MAX && divisor == -1)  return INT_MIN;
	    if (dividend <= INT_MIN && divisor == 1) return INT_MIN;
	    
		if (a < 0) { sign = -sign; a = -a; }
		if (b < 0) { sign = -sign; b = -b; }

		long long rst = 0;
		long long bitv[32] = {b};
		long long bit[32] = {1};

		int i = 1;
		for (; true; i++) {
			bitv[i] = bitv[i-1] + bitv[i-1];
			bit[i] = bit[i-1] + bit[i-1];
			if (a < bitv[i]) break;
		}

		long long remain = a;
		for (; i >= 0; i--) {
			if (bitv[i] <= remain) {
				remain -= bitv[i];
				rst += bit[i];
				if (remain == 0) break;
			}
		}

		if (sign < 0) rst = -rst;
		return rst;
	}

(2) 从最低位开始计算

	int divide(int dividend, int divisor) {
		int rlt = 0, flag = 1;
		int sign = 1;

		if (dividend <= INT_MIN && divisor == -1) return INT_MAX;
		if (dividend >= INT_MAX && divisor == -1)  return INT_MIN;
		if (dividend <= INT_MIN && divisor == 1) return INT_MIN;

		long long a = dividend;  
		long long b = divisor; 

		if (a < 0) { sign = -sign; a = -a; }
		if (b < 0) { sign = -sign; b = -b; }

		while ( a >= b) {
		    long long c = b;  
		    for(int i = 0; a >= c; i++, c <<=1)  
		    {  
		        a -= c; 
		        rlt += 1 << i; 
		    }  
		}

		if (sign < 0) rlt = -rlt;
		return rlt;
	}

### ** Single Number **
问题
Given an array of integers, every element appears twice except for one. Find that single one. Time limit O(n), space limit O(1).

思路：
因为除了特殊元素外，其他元素均出现两次。根据异或操作的规则可知：
a^b = b^a; 0^a = a;
所以，可得到
a^b^a^b^c = (a^a)^(b^b)^c = 0^0^c = c
则只要出现偶数次的数字，异或之后便为0，剩下的结果为数组中出现为单数的那个特殊数字。

	int singleNumber(int* nums, int numsSize) {
	    if (numsSize <= 0) return 0;
	    
	    int rst = nums[0];
	    for (int i = 1; i < numsSize; i++) {
	        rst ^= nums[i];
	    }
	    
	    return rst;
	}

### ** Single Number II **
问题
Given an array of integers, every element appears three times except for one. Find that single one.

思路：
通用方法
	数字按位计算; 数组中的所有元素都按位求和。求出每位对应的和之后，与3取模。如果使用位的数字在数组中出现的次数为3的倍数，则模三之后该位的结果为0，如果不是3的倍数，说明使用该位的元素中有出现次数不是3次的，所有这些位的十进制数为该特殊数字。

	int singleNumber(int* nums, int numsSize) {
	    if (numsSize <= 0) return 0;
	    
	    int intLen = sizeof(int) * 8, sum, rst = 0;
	    
	    for (int i = 0; i < intLen; i++) {
	        sum = 0;
	        for (int j = 0; j < numsSize; j++) {
	            sum += (nums[j] >> i) & 0x01;
	        }
	        rst += (sum % 3) << i;
	    }
	    return rst;
	}
### ** Single Number III **
问题：
Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.
For example:
Given nums = [1, 2, 1, 3, 2, 5], return [3, 5].

思路:
转换成两个Single Number I问题。
第一步，先求出整个数组的异或,也即求得两个特殊数字的异或
第二步，异或值代表两个数之间的差异。而且异或值中为1的位代表两个数字在该位上是不同的。因而可以根据这个位，把数组分成两个分组。此时问题则转换成两个Single Number I问题。
第三步，两个分组中，分别求出这两个值。

	int* singleNumber(int* nums, int numsSize, int* returnSize) {
	    
	    int* rst = (int*)malloc(sizeof(int)*2);
	    
	    int d = 0;
	    for (int i = 0; i < numsSize; i++) d ^= nums[i];
	    int lowestOneBit = d & -d;
	    
	    rst[0] = 0; rst[1] = 0;
	    for (int i = 0; i < numsSize; i++) {
	        int n = nums[i];
	        if (lowestOneBit && lowestOneBit & n) rst[0] = rst[0] ^ n; 
	        else rst[1] = rst[1] ^ n;
	    }
	    
	    return rst;
	}

### ** Majority Element **
Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.
You may assume that the array is non-empty and the majority element always exist in the array.

思路：
(1) 贪心法求解
	出现次数最多的数字必定是数组中二进制表示的数字，对应的每一位中的出现最多的，即如果数组中所有的该对应位大多数为1，则该位取1，反则，反之。把每一位出现最多的数字拼接到一起组成的数字便是出现次数最多的数字。

	int majorityElement(int* nums, int numsSize) {
	    int intLen = sizeof(int) * 8, rst = 0, flag = 1;
	    
	    for (int i = 0; i < intLen && flag; i++) {
	        int zNum = 0, oNum = 0;
	        for (int j = 0; j < numsSize; j++) {
	            int n = nums[j];
	            
	            if (n >> i & 0x01) oNum++; 
	            else zNum++; 
	        }
	        
	        if (oNum > zNum) rst += 1 << i;
	    }
	    
	    return rst;
	}
(2) Moore’s 投票算法
	见参考二

### ** Bitwise AND of Numbers Range **
问题:
Given a range [m, n] where 0 <= m <= n <= 2147483647, return the bitwise AND of all numbers in this range, inclusive.For example, given the range [5, 7], you should return 4.

解一:
思路：
[m, n]范围内的数字按位与，并将每位的结果求和。
先计算n的最高位。从高位到低位依次计算这些数字的对应位的与，遇到相应位的与不为1，则返回之前位的和。
注意：
最大整数范围，不可越界(j > INT_MIN)
截断，如果计算某位的时候结果为0，跳出循环

做完之后，反复看了一下，自己笑了。居然费这么大劲算了一些完全需要的东西。还为了不超时，反复思考如何提高效率。
不多说了，贴出来给大家笑一下

	int rangeBitwiseAnd(int m, int n) {
	    if (m == 0) return 0;
	    int rst = 0, bits = -1;
	    int k = n;
	    while (k) { bits++; k >>= 1;} 
	    
	    for (int i = bits; i >= 0; i--) {
	        int tmp = m >> i & 0x01;
	        for (int j = m+1 ; j > INT_MIN && j <= n ; j++) {
	            tmp &= j >> i;
	            if(!tmp) break;
	        }
	        if (i == bits && !tmp) return 0;
	        rst += tmp << i;
	    }
	    
	    return rst;
	}

解二:
思路：
只需要找到m, n两个数字相同的高位数字。

	int rangeBitwiseAnd(int m, int n) {

	   int bits = 0;
	   while (m != n) {
	         m >>= 1;
	         n >>= 1;
	         bits++;
	   }
	   
	   return m << bits;   
	}


### ** Maximum Product of Word Lengths **
问题
Given a string array words, find the maximum value of length(word[i]) * length(word[j]) where the two words do not share common letters. You may assume that each word will contain only lower case letters. If no such two words exist, return 0.
Example 1:
Given ["abcw", "baz", "foo", "bar", "xtfn", "abcdef"]
Return 16
The two words can be "abcw", "xtfn".
Example 2:
Given ["a", "aa", "aaa", "aaaa"]
Return 0
No such pair of words.

基本思路
如果使用brute force方法，复杂度为O(n<sup>2</sup>k<sup>2</sup>)其中k为数组中字符串的平均长度。
基本优化方法：把字符串转换成一个数字，通过数字的与运算，来判定是不是有相同的字符。因为只含有26个字符，所有可以采用整数来存放。而且相同的字符对应该二进制表示的整数中的相同的位。用1 << (char - 'a')转换成整数。x，y分别表示两个装换来的整数，x&y == 0 则表示没有相同的字符，如果x&y != 0表示不同的字符

space O(n)  只保存字符串转换成的整数，在计算字符串长度乘积时采用brure force，两层循环

	int maxProduct(char** words, int wordsSize) {
	    int rst = 0;
	    int* map = (int*)malloc(sizeof(int)*wordsSize);
	    memset(map, 0, sizeof(map));
	    
	    for (int i = 0; i < wordsSize; i++) {
	        char* word = words[i];
	        int bit = 0;
	        for (int j = 0; j < strlen(word); j++)  bit |= 1 << (word[j] - 'a');
	        map[i] = bit;
	    }
	   
	    for (int i = 0; i < wordsSize - 1; i++) {
	        for (int j = i+1; j < wordsSize; j++) {
	            if (map[i] & map[j]) continue;
	            else {
	                 int tmp = strlen(words[i]) * strlen(words[j]);
	                 rst = rst < tmp ? tmp : rst; 
	            }
	        }
	    }
	    
	    return rst;
	}

优化：
保存重复计算的结果。如在计算乘积时，总共计算了O(n<sup>2</sup>) 次字符串的长度。保存之后开n个空间，字符串长度的计算降到n次。时间复杂度降低了一半


### ** Subsets **
Given a set of distinct integers, nums, return all possible subsets.
Note:
Elements in a subset must be in non-descending order.
The solution set must not contain duplicate subsets.

思路1：
利用整数的二进制表示中的从高位到低位分别表示数组中从左到右的数字选取或者不选取。如果为1, 表示选取; 如果为0, 表示不选取。但是有限制，如果数据超过64位该算法失效

	var subsets = function(nums) {

		var len = nums.length;
		var size = 1 << len;
		var  subs = [];

		nums.sort(function(a,b){return a-b;});
		for (var i = 0; i < size; i++) {
			var tmp = i;
			var sub = [];
			for (var j = 0; (j < len) && tmp; j++) {
				if (tmp & 1) sub.push(nums[j]);
				tmp >>= 1;
			}
			subs.push(sub);
		}

		return subs;
	};
思路2:
回溯算法

	var subsets = function(nums) {
		var rst = [], path = [];
		
		nums.sort(function(a, b){ return a-b; });
		dfs(nums, 0, path, rst);
		
		return rst;
	};
	var dfs = function(nums, k, path, rst) {
		if (k == nums.length) {
			rst.push(path.slice());
			return;
		}
		
		dfs(nums, k+1, path, rst);
		path.push(nums[k]);
		dfs(nums, k+1, path, rst);
		path.pop();
	}



### ** 位的常用操作： **
(1) 取二进制整数x中的第i(从0开始计)位 
	(x >> i) & 0x01 ((x >> i) & 1)
	取二进制整数x中从第i位开始的从右向左依次取i位
	(x >> i) & (具有i个1的二进制数)

(2) 求补码
	x为整数, -x为补码

(3) 求数字中, 从右向左, 第一个为1的位的位, 并返回该值
	x & -x

(4) x取反
	~(x+1)
参考：
http://blog.csdn.net/lcj_cjfykx/article/details/48531073
http://ju.outofmemory.cn/entry/230620

