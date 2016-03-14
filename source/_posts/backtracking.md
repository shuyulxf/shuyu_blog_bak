title: backtracking
tags: [learning, 算法, leetcode]
type: tags
date: 2016-03-03 18:56:28
---
### ** Generate Parentheses **
问题
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.
For example, given n = 3, a solution set is:
"((()))", "(()())", "(())()", "()(())", "()()()"

思路：
backtracking
<!--more-->
源码

	var generateParenthesis = function(n) {
	    var len = 2 * n;
	    var rst = [], path = "", pattern = ["(", ")"];

	    dfs(len, 0, pattern, path, rst);
		
		for(var i = 0; i < rst.length; i++) console.log(rst[i]);
		return rst;
	};
	var dfs = function(n, k, pattern, path, rst) {
		if (k == n) {
			if (isValid(path))	rst.push(path);
			return;
		}

		for (var i = 0; i < 2; i++) {
			path += pattern[i];
			dfs(n, k+1, pattern, path, rst);
			path = path.substring(0, path.length - 1);
		}
	}

	var isValid = function(s) {
	    var stack = [];

	    for (var i = 0; i < s.length; i++) {
	    	if (s[i] == "(")  stack.push(s[i]);
	    	if (s[i] == ")")  {
	    		if (stack.length)	stack.pop();
	    		else return false;
	    	}
	    }

	    if (!stack.length)  return true;
	    return false;
	};

### ** Letter Combinations of a Phone Number **
问题
Given a digit string, return all possible letter combinations that the number could represent.
A mapping of digit to letters (just like on the telephone buttons) is given below.
Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].

思路
backtracking

源码

	var letterCombinations = function(digits) {
	    var pattern = ["","","abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"];
	    var rst = [], path = "";

	    digits = digits.split("").filter(function(v){
	    	return v !== 0 && v !== 1;
	    });

	    if (!digits.length) return rst;

	    for (var i = 0; i < pattern.length; i++) {
	    	pattern[i] = pattern[i].split("");
	    }
	    dfs(digits, pattern, 0, path, rst);
	   // for(var i = 0; i < rst.length; i++) console.log(rst[i]);
	    return rst;
	};
	var dfs = function(digits, pattern, k, path, rst){
		if (k == digits.length) {
			rst.push(path);
			return;
		}
		var p = pattern[digits[k] - 0];
		
		for(var i = 0; i < p.length; i++) {
			path += p[i];
			dfs(digits, pattern, k+1, path, rst);
			path = path.substring(0, path.length - 1);
		}
	}