---
title: "位运算小结"
date: 2020-04-17T07:35:24+08:00
lastmod: 2020-04-17T07:35:24+08:00
draft: true
author: ""
authorLink: ""
description: ""
license: ""

tags: []
categories: []
hiddenFromHomePage: false

featuredImage: ""
featuredImagePreview: ""

toc: false
autoCollapseToc: true
math: false
lightgallery: true
linkToMarkdown: true
share:
  enable: true
comment: true
---

<!--more-->

## 声明最大值
```
const MaxUint = ^uint(0)
const MaxInt = int(^uint(0) >> 1)
const Is64bitArch = ^uint(0) >> 63 == 1
const Is32bitArch = ^uint(0) >> 63 == 0
const WordBits = 32 << (^uint(0) >> 63) // 64或32
```

## 加减乘除
### 加法
不考虑进位时，加法就是`a^b`，进位则是`(a&b)<<1`。
```
func add(a, b int) int {
    sum := a
    for b != 0 {
        sum = a^b
		b = (a&b)<<1
		a  = sum
    }
    return sum
}
```

### 减法
a-b = a+(-b)
```
func neg(a int) int {
    return add(^a, 1)
}

func minus(a, b int) int {
    return add(a, neg(b))
}
```

### 乘法
```
func multi(a, b int) int {
    res := 0
    for b != 0 {
        if b&1 != 0 {
            res = add(res, a)
        }
        a <<= 1
        b >>= 1
    }
    return res
}
```

### 除法
var (
    IntMax = int(^uint(0)>>1)
    IntMin = -IntMax - 1
)

func isNeg(a int) bool {
    return a < 0
}

func div(a, b int) int {
    resNeg := isNeg(a) ^ isNeg(b)
    if isNeg(a) {
        a = neg(a)
    }
    if isNeg(b) {
        b = neg(b)
    }
    res := 0
    for i:=31; i>=0; i=minus(i, 1) {
        if a>>i >= b {
            res |= (1<<i)
            a = minus(a, b<<i)
        }
    }
    if resNeg {
        return neg(res)
    }
    return res
}

func divide(a, b int) int {
    if b == 0 {
        panic("除数为0")
    }
    if a == IntMin && b == IntMin {
        return 1
    }
    if b == IntMin {
        return 0
    }
    if a == IntMin {
        res = div(add(a, 1), b)
        return add(res, div(minus(a, multi(res, b)), b))
    }
    return div(a, b)
}
## leetcode 位运算经典题

## 大数据算法题
hash＋bit map爆锤