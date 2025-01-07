+++
date = '2025-01-06T21:52:24+08:00'
draft = false
title = 'Golang-排序'
categories = ["golang"]
tags = ["golang"]
searchHidden = false
ShowToc = true
TocOpen = false
+++

## 一般排序
基本的sort套件。
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

	//sot int reverse
	sort.Sort(sort.Reverse(sort.IntSlice(s1)))
	fmt.Println(s1)

	//sort string
	s2 := []string{"aaa", "bbb", "6", "3", "1", "4"}
	sort.Strings(s2)
	fmt.Println(s2)

	//sot string reverse
	sort.Sort(sort.Reverse(sort.StringSlice(s2)))
	fmt.Println(s2)

	//sort float64
	s3 := []float64{1, 1.2, 0, -1.9, -82.333, 99.11}
	sort.Float64s(s3)
	fmt.Println(s3)

	//sot float64 reverse
	sort.Sort(sort.Reverse(sort.Float64Slice(s3)))
	fmt.Println(s3)
}
```

## 物件排序
物件列表指定參數排序。
```golang
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
## 二階排序
如果Name一樣時，希望再透過ID做二次排序。
```golang
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
	ds = append(ds, Data{ID: 13, Name: "kevin"})
	ds = append(ds, Data{ID: 12, Name: "kevin"})
	ds = append(ds, Data{ID: 15, Name: "lily"})

	//sort by Name
	sort.Slice(ds, func(i, j int) bool {
		if ds[i].Name == ds[j].Name {
			return ds[i].ID < ds[j].ID
		}
		return ds[i].Name < ds[j].Name
	})
	fmt.Println(ds)
}
```

## 自然排序
你是否常遇到以下的問題?
["A11","A3","A2","A1"]
字串排序結果: ["A1","A11","A2","A3"]
希望結果: ["A1","A2","A3","A4"]

IP的排序也會有此需求，這時候你需要自然排序!!!

引用套件:
```powershell
go get github.com/facette/natsort
```

```golang
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
	//sort string
	s2 := []string{"Device2", "Device11", "Device1", "Device22", "Device13", "Device3"}
	sort.Strings(s2)
	fmt.Println(s2)

	//natsort string
	s2 = []string{"Device2", "Device11", "Device1", "Device22", "Device13", "Device3"}
	natsort.Sort(s2)
	fmt.Println(s2)

	//natsort struct slice
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