+++
date = '2025-01-06T21:52:24+08:00'
draft = false
title = 'Golang-sort'
categories = ["golang"]
tags = ["golang"]
+++

Looking for efficient ways to sort data in Go? This guide covers everything from basic sorting to advanced techniques!

## 🚀 Basic Sorting

The `sort` package provides fundamental sorting capabilities.

```golang
package main

import (
    "fmt"
    "sort"
)

func main() {
    //sort int
    s1 := []int{8, 2, 6, 3, 1, 4}
    sort.Ints(s1)
    fmt.Println(s1)

    //sort int reverse
    sort.Sort(sort.Reverse(sort.IntSlice(s1)))
    fmt.Println(s1)

    //sort string
    s2 := []string{"aaa", "bbb", "6", "3", "1", "4"}
    sort.Strings(s2)
    fmt.Println(s2)

    //sort string reverse
    sort.Sort(sort.Reverse(sort.StringSlice(s2)))
    fmt.Println(s2)

    //sort float64
    s3 := []float64{1, 1.2, 0, -1.9, -82.333, 99.11}
    sort.Float64s(s3)
    fmt.Println(s3)

    //sort float64 reverse
    sort.Sort(sort.Reverse(sort.Float64Slice(s3)))
    fmt.Println(s3)
}
``` 
## 🎯 Object Sorting
How to sort a list of objects by specific fields:

``` golang

package main

import (
    "fmt"
    "sort"
)

type Data struct {
    ID   int
    Name string
}

func main() {
    //sort struct
    ds := []Data{}
    ds = append(ds, Data{ID: 49, Name: "kevin"})
    ds = append(ds, Data{ID: 11, Name: "peter"})
    ds = append(ds, Data{ID: 11, Name: "mary"})
    ds = append(ds, Data{ID: 11, Name: "adon"})
    ds = append(ds, Data{ID: 15, Name: "lily"})

    //sort by id
    sort.Slice(ds, func(i, j int) bool {
        return ds[i].ID < ds[j].ID
    })
    fmt.Println(ds)

    //sort by Name
    sort.Slice(ds, func(i, j int) bool {
        return ds[i].Name < ds[j].Name
    })
    fmt.Println(ds)

    //sort by id reverse
    sort.Slice(ds, func(i, j int) bool {
        return ds[i].ID > ds[j].ID
    })
    fmt.Println(ds)
}
``` 
## 🎨 Multi-Level Sorting
Need secondary sorting when primary fields are equal? Here's how:

``` golang
package main

import (
    "fmt"
    "sort"
)

type Data struct {
    ID   int
    Name string
}

func main() {
    ds := []Data{}
    ds = append(ds, Data{ID: 49, Name: "kevin"})
    ds = append(ds, Data{ID: 13, Name: "kevin"})
    ds = append(ds, Data{ID: 12, Name: "kevin"})
    ds = append(ds, Data{ID: 11, Name: "peter"})
    ds = append(ds, Data{ID: 15, Name: "lily"})

    //sort by Name, then by ID
    sort.Slice(ds, func(i, j int) bool {
        if ds[i].Name == ds[j].Name {
            return ds[i].ID < ds[j].ID
        }
        return ds[i].Name < ds[j].Name
    })
    fmt.Println(ds)
}
``` 
## ⭐ Natural Sorting
Ever faced issues sorting strings with numbers?
For example:

Input: ["A11", "A3", "A2", "A1"]
Standard sort result: ["A1", "A11", "A2", "A3"]
Desired result: ["A1", "A2", "A3", "A11"]
Natural sorting solves this problem perfectly! It's especially useful for:
  
Version numbers  
IP addresses  
File names with numbers

### Installation
First, install the required package:

``` bash
go get github.com/facette/natsort
``` 

Here's how to use natural sorting:

``` golang
package main

import (
    "fmt"
    "sort"
    "github.com/facette/natsort"
)

type Data struct {
    ID   string
    Name string
}

func main() {
    // Standard string sort comparison
    s2 := []string{"Device2", "Device11", "Device1", "Device22", "Device13", "Device3"}
    sort.Strings(s2)
    fmt.Println("Standard sort:", s2)

    // Natural sort
    s2 = []string{"Device2", "Device11", "Device1", "Device22", "Device13", "Device3"}
    natsort.Sort(s2)
    fmt.Println("Natural sort:", s2)

    // Natural sort with structs
    ds := []Data{}
    ds = append(ds, Data{ID: "2", Name: "kevin"})
    ds = append(ds, Data{ID: "11", Name: "peter"})
	ds = append(ds, Data{ID: "1", Name: "mary"})
	ds = append(ds, Data{ID: "3", Name: "adon"})
	ds = append(ds, Data{ID: "12", Name: "lily"})
	//sort by id
	sort.Slice(ds, func(i, j int) bool {
		return natsort.Compare(ds[i].ID, ds[j].ID)
	})

	fmt.Println(ds)
}
``` 