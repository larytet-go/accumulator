# accumulator

This is a fast, thread safe accumulator. The implementation is based on a cyclic buffer. 
Application calls Add() to add a value to the accumulator. Application calls Tick() to move to the next entry.

	package main

	import (
		"fmt"
		"github.com/larytet-go/accumulator"
		"time"
	)

	func main() {
		// Sliding window with 32 items
		rate := accumulator.New("rate", 32)
		// Move the sliding window by one every second
		go func() {
			for {
				time.Sleep(1 * time.Second)
				rate.Tick()
			}
		}()

		go func() {
			for {
				rate.Add(1)
				time.Sleep(10 * time.Millisecond)
			}
		}()

		// I expect the counters to be in the 99-101 range
		time.Sleep(32 * time.Second)
		s := rate.Sprintf("%-28s:\n%v\n", "%-28sNo requests in the last %d seconds\n", "%8d ", 16, 1, false)
		fmt.Print(s)
	}
