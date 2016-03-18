---
title: 链表逆序
date: 2016-03-18 14:56:34
tags: [算法, js]
---

** 递归解法 **


	var reverse = function(p, q){
	    if (q == null)  return p;
	    var nq = q.next;
	    q.next = p;
	    return reverse(q, nq);
	}


** 非递归解法 **


	var reverse = function(head) {
	    var pre = new ListNode(-1), i = 0;
	    var h = null;

	    while (head) {
	        var tmp = pre.next;
	        pre.next = head;
	        head = head.next;
	        pre.next.next =tmp;
	    }

	    return pre.next;
	}
