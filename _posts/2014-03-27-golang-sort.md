---
layout: post
title: "golang 排序"
description: ""
category: 
tags: []
intro: "golang 排序"
---

golang srot包

附上例子

	 type UserInfo struct {
		WinNum    int
		FightLift int
	}

	type MapSorter []UserInfo

	func NewMapSorter() MapSorter {
		ms := make(MapSorter, 0, 2)
		ms = append(ms, UserInfo{1, 5})
		ms = append(ms, UserInfo{4, 3})
		ms = append(ms, UserInfo{3, 3})
		ms = append(ms, UserInfo{5, 3})
		ms = append(ms, UserInfo{4, 2})
		return ms
	}

	func (ms MapSorter) Len() int {
		return len(ms)
	}

	func (ms MapSorter) Less(i, j int) bool {
		return ms[i].WinNum > ms[j].WinNum
	}

	func (ms MapSorter) Swap(i, j int) {
		ms[i], ms[j] = ms[j], ms[i]
	}

	type MapSorterByFightLift struct {
		MapSorter
	}

	func (s MapSorterByFightLift) Less(i, j int) bool {
		return s.MapSorter[i].FightLift < s.MapSorter[j].FightLift
	}
         
	 //输出
	 ms := NewMapSorter()
	sort.Sort(ms)
	sort.Sort(MapSorterByFightLift{ms})
	//选FightLift升序，相同的就WinNum降序
	fmt.Println( ms)、、[{4 2} {5 3} {4 3} {3 3} {1 5}]