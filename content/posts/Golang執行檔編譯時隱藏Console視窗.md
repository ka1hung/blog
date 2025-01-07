---
title: Golang執行檔編譯時隱藏Console視窗
date: 2024-03-20 10:37:55
categories: golang
tags: 
 - windows 
 - golang
---

``` powershell
cd to/your/project/
go build -ldflags -H=windowsgui .
```