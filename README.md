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

The Set implementation is designed to work concurrently from multiple Goroutines. Therefore, if you try to add an element
to a full Set, the Add function will block as long as the Set is full:
```go
s := capacityset.NewCapacitySet(3)
s.Add("banana") // true
s.Add("peach") // true
s.Add("apple") // true
go func() {
    time.Sleep(time.Second*3) // simulates work until the Pop function is called
    s.Pop() // either banana, peach or apple
}()
before := time.Now()
s.Add("coconut") // blocks until the set is no longer full
fmt.Println(time.Since(before)) // would print round about "3s"
```

Please note that if you try to add an element to a full Set within the same Goroutine, the Add function panics because
Golang detects a deadlock.

## Contributing
Refer to [CONTRIBUTING.md](CONTRIBUTING.md).
