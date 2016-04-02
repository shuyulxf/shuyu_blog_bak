---
title: Substring with Concatenation of All Words
date: 2016-03-18 23:57:33
tags: [算法,js,leetcode]
---
Substring with Concatenation of All Words
问题:
You are given a string, s, and a list of words, words, that are all of the same length. Find all starting indices of substring(s) in s that is a concatenation of each word in words exactly once and without any intervening characters.
For example, given:
s: "barfoothefoobarman"
words: ["foo", "bar"]
You should return the indices: [0,9].

解法一:
思路:
最直接的就是想到用回溯算法找到words数组的全排列，然后用KMP算法进行字符串匹配。
但是这样做，在words长度很大的时候，无法得到words数组的全排列。
虽然对于长度很大的words,回溯法求解不出所有的组合情况，是该实现的限制所在，还是把代码贴出来。


	var findSubstring = function(s, words) {
	    var rlt = [], ids = [];
	    var al = words.length;
	    allSort(words, rlt, 0, al);
	    
	    for (var i = 0; i < rlt.length; i++) {
	        var t = kmp(s, rlt[i]);
	        if (t != -1) ids.push(t);
	    }

	    return ids;
	};

	var allSort = function(arr, rlt, k, l){
	    if (k == l) {
	        var tmp = arr.join("");
	        if(isDup(rlt, tmp)) rlt.push(arr.join(""));
	        return;
	    }

	    for (var i = k; i < l; i++) {
	        swap(arr, i, k);
	        allSort(arr, rlt, k+1, l);
	        swap(arr, i, k);
	    }
	}
	var isDup = function(rlt, itm){
	    for (var i = 0; i < rlt.length; i++) {
	        if(rlt[i] == itm.toString())   return false;
	    }

	    return true;
	}
	var swap = function(arr, x, y) {
	    var tmp = arr[x];
	    arr[x] = arr[y];
	    arr[y] = tmp;
	}

	var kmp = function(haystack, needle) {
	    
	    var l1 = haystack.length, l2 = needle.length;
	    if (l1 < l2) return -1;
	    if (!l2) return 0;
	    var next = getNext(needle);

	    for (var i = 0, j = 0; i < l1; i++) {
	        while (j > 0 && haystack[i] != needle[j])   j = next[j-1];
	        if (haystack[i] == needle[j]) j++;
	        if (j == l2)    return i - j + 1;
	    }

	    return -1;
	};
	function getNext(s) {
	    var len = s.length;
	    var next = []; 
	    next.push(0);

	    for (var i = 1, j = 0; i < len; i++) {
	        while (j > 0 && s[i] != s[j]) j = next[j-1];
	        if (s[i] == s[j])   j++;
	        next[i] = j;
	    }

	    return next;
	}