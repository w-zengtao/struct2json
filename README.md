# Struct2Json

[![go report card](https://goreportcard.com/badge/github.com/w-zengtao/struct2json "go report card")](https://goreportcard.com/report/github.com/w-zengtao/struct2json)
[![MIT license](http://img.shields.io/badge/license-MIT-brightgreen.svg)](http://opensource.org/licenses/MIT)

`Struct2Json` offers the ability to convert `Struct` to `map[string]interface{}` directly

I support `json` tag & `noroot` tag to help us organize our output `JSON`

see examples below

## Installation

```shell
go get github.com/w-zengtao/struct2json
```

## How to use

```go
type AStruct struct {
	A int `json:"a"`
}

type BStruct struct {
	B int `json:"b"`
}

type CStruct struct {
	C        int
	D        *int
	E        *int
	AS       AStruct `json:"as"`
	BS       BStruct `json:"bs"`
	ASP      *AStruct
	BSP      *BStruct
	ASNoroot AStruct `json:"asnoroot" noroot:"true"`
}

var (
	i = 10
)

input := CStruct{
  C: 1,
  D: &i,
  AS: AStruct{
    A: 1,
  },
  BSP: &BStruct{
    B: 1,
  },
  ASNoroot: AStruct{
    A: 100,
  },
}

struct2json.Convert(input)
```

## output

```javascript
{
	ASP: nil, // nil pointer
	BSP: {
		b: 1
	},
	C: 1,
	D: 10,  // *int
	E: nil,
	a: 100, // ASNoroot
	as: {
		a: 1
	},
	bs: {
		b: 0
	}
}
```

## traps

* Only `pointer` can have nil value, so if you do not want the `Go's default value`, use pointer filed.
* Only `struct` or `pointer to struct` support `noroot` tag
* Once `noroot` meet same json tag, a `replace` action will be happend

## License

© w-zengtao, 2019~time.Now

Released under the [MIT License](https://github.com/w-zengtao/struct2json/blob/master/License)