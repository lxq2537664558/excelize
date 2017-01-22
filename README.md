![Excelize](./excelize.png "Excelize")

# Excelize

[![Build Status](https://travis-ci.org/Luxurioust/excelize.svg?branch=master)](https://travis-ci.org/Luxurioust/excelize)
[![Code Coverage](https://codecov.io/gh/Luxurioust/excelize/branch/master/graph/badge.svg)](https://codecov.io/gh/Luxurioust/excelize)
[![Go Report Card](https://goreportcard.com/badge/github.com/Luxurioust/excelize)](https://goreportcard.com/report/github.com/Luxurioust/excelize)
[![GoDoc](https://godoc.org/github.com/Luxurioust/excelize?status.svg)](https://godoc.org/github.com/Luxurioust/excelize)
[![Licenses](https://img.shields.io/badge/license-bsd-orange.svg)](https://opensource.org/licenses/BSD-3-Clause)
[![Join the chat at https://gitter.im/xuri-excelize/Lobby](https://img.shields.io/badge/GITTER-join%20chat-green.svg)](https://gitter.im/xuri-excelize/Lobby)

## Introduction

Excelize is a library written in pure Golang and providing a set of functions that allow you to write to and read from XLSX files. Support reads and writes XLSX file generated by Office Excel 2007 and later. Support save file without losing original charts of XLSX. The full API docs can be seen using go's built-in documentation tool, or online at [godoc.org](https://godoc.org/github.com/Luxurioust/excelize).

## Basic Usage

### Installation

```go
go get github.com/Luxurioust/excelize
```

### Create XLSX files

Here is a minimal example usage that will create XLSX file.

```go
package main

import (
    "fmt"
    "os"

    "github.com/Luxurioust/excelize"
)

func main() {
    xlsx := excelize.CreateFile()
    xlsx.NewSheet(2, "Sheet2")
    xlsx.NewSheet(3, "Sheet3")
    xlsx.SetCellInt("Sheet2", "A23", 10)
    xlsx.SetCellStr("Sheet3", "B20", "Hello")
    err := xlsx.WriteTo("/tmp/Workbook.xlsx")
    if err != nil {
        fmt.Println(err)
        os.Exit(1)
    }
}
```

### Writing XLSX files

The following constitutes the bare minimum required to write an XLSX document.

```go
package main

import (
    "fmt"
    "os"

    "github.com/Luxurioust/excelize"
)

func main() {
    xlsx, err := excelize.OpenFile("/tmp/Workbook.xlsx")
    if err != nil {
        fmt.Println(err)
        os.Exit(1)
    }
    xlsx.SetCellValue("Sheet2", "B2", 100)
    xlsx.SetCellValue("Sheet2", "C7", "Hello")
    xlsx.NewSheet(4, "TestSheet")
    xlsx.SetCellInt("Sheet4", "A3", 10)
    xlsx.SetCellStr("Sheet4", "b6", "World")
    xlsx.SetActiveSheet(2)
    err = xlsx.Save()
    if err != nil {
        fmt.Println(err)
        os.Exit(1)
    }
}
```

### Reading XLSX files

```go
package main

import (
    "fmt"
    "os"

    "github.com/Luxurioust/excelize"
)

func main() {
    xlsx, err := excelize.OpenFile("/tmp/Workbook.xlsx")
    if err != nil {
        fmt.Println(err)
        os.Exit(1)
    }
    cell := xlsx.GetCellValue("Sheet2", "C7")
    fmt.Println(cell)
}
```

### Add picture to XLSX files

```go
package main

import (
    "fmt"
    "os"
    _ "image/gif"
    _ "image/jpeg"
    _ "image/png"

    "github.com/Luxurioust/excelize"
)

func main() {
    xlsx := excelize.CreateFile()
    // Insert a picture.
    err := xlsx.AddPicture("Sheet1", "A2", "/tmp/image1.jpg", 0, 0, 1, 1)
    // Insert a picture to sheet with scaling.
    err = xlsx.AddPicture("Sheet1", "D2", "/tmp/image1.png", 0, 0, 0.5, 0.5)
    // Insert a picture offset in the cell.
    err = xlsx.AddPicture("Sheet1", "H2", "/tmp/image3.gif", 15, 10, 1, 1)
    if err != nil {
        fmt.Println(err)
        os.Exit(1)
    }
    err = xlsx.WriteTo("/tmp/Workbook.xlsx")
    if err != nil {
        fmt.Println(err)
        os.Exit(1)
    }
}
```

## Contributing

Contributions are welcome! Open a pull request to fix a bug, or open an issue to discuss a new feature or change.

## Credits

Some struct of XML originally by [tealeg/xlsx](https://github.com/tealeg/xlsx).

## Licenses

This program is under the terms of the BSD 3-Clause License. See [https://opensource.org/licenses/BSD-3-Clause](https://opensource.org/licenses/BSD-3-Clause).
