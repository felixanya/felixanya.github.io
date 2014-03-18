---
layout: post
title: "golang 高阶函数"
description: ""
category: 
tags: []
intro: "golang 高阶函数"
---

今晚睡不着，写了一个函数

附上例子

	 // 查找数组
	 func SliceIndex(limit int, predicate func(i int) bool) int {
	      for i := 0; i < limit; i++ {
		 if predicate(i) {
		      return i
		 }
	      }
	      return -1
	 }

	 xs := []int{2, 4, 6, 8}
	 ys := []string{"C", "B", "K", "A"}
	 a := SliceIndex(len(xs), func(i int) bool { return xs[i] == 6 })
	 b := SliceIndex(len(ys), func(i int) bool { return ys[i] == "Z" })
         
	 //输出
	 fmt.Println("a", a)//2
	 fmt.Println("b", b)//-1


	//过滤奇数
	func Fileter(limit int, predicate func(int) bool, appender func(int)) {
	  for i := 0; i < limit; i++ {
	   if predicate(i) {
	     appender(i)
	   }
	 }
	}

	xs := []int{2, -3, -74, 6, 8, 9, 11}
	even := make([]int, 0, len(xs))
	Fileter(len(xs), func(i int) bool { return xs[i]%2 == 0 }, func(i int) { even = append(even, xs[i]) })

        //输出
	fmt.Println("even", even)//even [2 -74 6 8]