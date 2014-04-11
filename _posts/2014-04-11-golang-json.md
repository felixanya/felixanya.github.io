---
layout: post
title: "golang json 特别说明"
description: ""
category: 
tags: []
intro: "golang json 特别说明"
---

JSON 结构体的特别说明
json的object的key是项名,但也可以使用项的标签(json struct tag)
 
//忽略此项

Field int `json:"-"`

//json中的key是myName

Field int `json:"myName"`

//json中的key是myName,值为零则忽略

Field int `json:"myName,omitempty"`

//json中的key是Field,值为零则忽略

Field int `json:",omitempty"`