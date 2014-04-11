---
layout: post
title: "golang json 特别说明"
description: ""
category: 
tags: []
intro: "golang json 特别说明"
---

JSON 结构体的特别说明

### json生成说明
- json的object的key是项名,但也可以使用项的标签(json struct tag)
- 特别注意 空指针会输出null 所以切片就为null

### json解析说明
- 要解析的字符串比struct要多字段,不会出错(因为不解析)
- 要解析的字符串比struct要少字段,会输出默认值

//忽略此项

Field int `json:"-"`

//json中的key是myName

Field int `json:"myName"`

//json中的key是myName,值为零则忽略

Field int `json:"myName,omitempty"`

//json中的key是Field,值为零则忽略

Field int `json:",omitempty"`


附上例子

	package test

	import (
		"encoding/json"
		"fmt"
		"testing"
	)

	type Response struct {
		Id     int      `json:"id,omitempty"`
		Name   string   `json:"name,omitempty"`
		Fruits []string `json:"fruits"`
	}

	func TestClass(t *testing.T) {
		res2D := &Response{
			Id:     0,
			Name:   "felix",
			Fruits: []string{"apple", "peach", "pear"}}
		res2B, _ := json.Marshal(res2D)
		//输出{"name":"felix","fruits":["apple","peach","pear"]}
		fmt.Println(string(res2B)) 

		res2D = &Response{
			Id:     10,
			Name:   "",
			Fruits: []string{"apple", "peach", "pear"}}
		res2B, _ = json.Marshal(res2D)
		//输出{"id":10,"fruits":["apple","peach","pear"]}
		fmt.Println(string(res2B)) 

		res2D = &Response{
			Fruits: []string{"apple", "peach", "pear"}}
		res2B, _ = json.Marshal(res2D)
		//输出{"fruits":["apple","peach","pear"]}
		fmt.Println(string(res2B)) 

		res2D = &Response{
			Id:   10,
			Name: "felix"}
		res2B, _ = json.Marshal(res2D)
		//输出{"id":10,"name":"felix","fruits":null}
		fmt.Println(string(res2B)) 


		var s Response
		str := `{"test":"good","name":"felix","fruits":["apple","peach","pear"]}`
		err := json.Unmarshal([]byte(str), &s)
		//输出{0 felix [apple peach pear]} <nil>
		fmt.Println(s, err)
	}
