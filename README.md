# capacityset

[![Build Status](https://travis-ci.com/xmidt-org/capacityset.svg?branch=master)](https://travis-ci.com/xmidt-org/capacityset)
[![codecov.io](http://codecov.io/github/xmidt-org/capacityset/coverage.svg?branch=master)](http://codecov.io/github/xmidt-org/capacityset?branch=master)
[![Code Climate](https://codeclimate.com/github/xmidt-org/capacityset/badges/gpa.svg)](https://codeclimate.com/github/xmidt-org/capacityset)
[![Issue Count](https://codeclimate.com/github/xmidt-org/capacityset/badges/issue_count.svg)](https://codeclimate.com/github/xmidt-org/capacityset)
[![Go Report Card](https://goreportcard.com/badge/github.com/xmidt-org/capacityset)](https://goreportcard.com/report/github.com/xmidt-org/capacityset)
[![Apache V2 License](http://img.shields.io/badge/license-Apache%20V2-blue.svg)](https://github.com/xmidt-org/capacityset/blob/master/LICENSE)
[![GitHub release](https://img.shields.io/github/release/xmidt-org/capacityset.svg)](CHANGELOG.md)
[![GoDoc](https://godoc.org/github.com/xmidt-org/capacityset?status.svg)](https://godoc.org/github.com/xmidt-org/capacityset)

## Summary

Capacityset implements a set (unordered, non-repeating) that cannot exceed a 
provisioned size.

## Install
This repo is a package. To install it, just run the `go get` command from your command line:
```sh
go get -u github.com/xmidt-org/capacityset
```

## Examples

Creating a Set and adding values is very easy. You can not add duplicates to the set:
```go
s := capacityset.NewCapacitySet(3)
// the Add method returns a boolean indicating if the operation was successful
s.Add("banana") // true
s.Add("peach") // true
fmt.Println(s.Add("banana")) // prints false because the value is already present in the Set
```

In order to retrieve values from the Set, you can use the `Pop` function. In addition to that, the `Size` function helps 
you to keep track of the current size of the Set:
```go
s := capacityset.NewCapacitySet(3)
s.Add("banana") // true
s.Add("peach") // true
s.Add("apple") // true
fmt.Println(s.Size()) // prints 3
// the Pop method returns a random element and removes it from the Set
fmt.Println(s.Pop()) // either banana, peach or apple
fmt.Println(s.Size()) // prints 2
```

If you try to add an element to a Set and exceed the Set's capacity, the `Add` function will panic:
```go
s := capacityset.NewCapacitySet(3)
s.Add("banana") // true
s.Add("peach") // true
s.Add("apple") // true
s.Add("coconut") // would panic because it exceeds the Set's capacity
```

## Contributing
Refer to [CONTRIBUTING.md](CONTRIBUTING.md).
