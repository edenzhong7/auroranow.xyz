---
title: "Go技巧101"
date: 2020-03-29T08:34:21+08:00
lastmod: 2020-03-29T08:34:21+08:00
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

## 边界检查消除（Bounds Check Elimination）
在Go的array/slice中取值时会触发边界检查，判断下标是否越界。从Go SDK 1.7开始，官方标准编译器使用了一个新的基于SSA(single-assignment form，静态单赋值形式)的后端，可有效利用BCE优化。
检测方式:
```
go build -gcflags="-d=ssa/check_bce/debug=1"
```

举个🌰:
```
a := arr[1] // bc
b := arr[0] // bce
比
a := arr[0] // bc
b := arr[1]  // bc
```

看着是使用了前面边界检查的结果推理后面的下标是否有效，举个实际应用的🌰:
```
func isSubString(a, b string) bool {
    if len(a) > len(b) {
        a, b = b, a
    }
    for i, r := range a {
        if r != b[i] {         // bc
            return false
        }
    }
    return true
}

// 以下代码虽然冗余，但是却减少了BC的次数
func isSubString(a, b string) bool {
    if len(a) > len(b) {
        a, b = b, a
    }
    if len(a) <= len (b) {   // true， 只需要在这bc一次
        for i, r := range a { 
            if r != b[i] {     // bce
                return false
            }
        }
    }
   
    return true
}
```
but，有的地方bce却不像期望那样生效，有bug？？?
```
func fa() {
    s := []int{0, 1, 2, 3, 4, 5, 6}
    index := rand.Intn(7)
    _ = s[:index] // bc
    _ = s[index:] // bce
}


func fb(s []int, index int) {
    _ = s[:index] // bc
    _ = s[index:] // 还是要bc???
}

func fc() {
    s := []int{0, 1, 2, 3, 4, 5, 6}
    s = s[:4]
    index := rand.Intn(7)
    _ = s[:index] // bc
    _ = s[index:] // bc ???
}
```
其实这是因为s[a:b]中b需要满足"0 <= a <= b <= cap(s)"，而a需要满足"0 <= a <= len(s)"，所以b在下标有效不能推导出b在左下标也有效。只有在len和cap相同的情况下，才能从s[:index]推导出s[index:]是正确的。

## Slice三下标
完整的slice下标写法应该是`sub := input[low:high:max]`，常用写法中忽略了`max`，默认值是`cap(input)`，其中`len(sub) = high-low, cap(sub) = max-low`。当使用子切片作为函数传递又不希望除此之外的元素被修改时可以令`max=high`。举个🌰:
```
package main
import "fmt"

func fa(a []int) {
    a = append(a, 3)
    fmt.Printf("%#v\n", a)
}

func fb(b []int) {
    b = append(b, 3) // 因为len(b)=cap(b), 所以append重新分配了一个底层数组并把b拷贝过去
    fmt.Printf("%#v\n", b)
}

func main() {
    a := []int{1, 10, 100}
    b := []int{1, 10, 100}
    fa(a[:2])
    fb(b[:2:2])
    println("*****")
    fmt.Printf("%#v\n", a)
    fmt.Printf("%#v\n", b)
}
/* output:
[]int{1, 10, 3}
[]int{1, 10, 3}
*****
[]int{1, 10, 3}     // a被修改了
[]int{1, 10, 100}   // b没被修改了
*/
```