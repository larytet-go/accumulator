# accumulator

This is a fast, thread safe accumulator. The implementation is based on a cyclic buffer. 
Application calls Add() to add a value to the accumulator. Application calls Tick() to move to the next entry.

	import (
		"github.com/larytet-go/accumulator"
	)

	func main() {
		rate := accumulator.New("rate", 32)
		go func() {
		  time.Sleep(1*time.Second)
		  rate.Tick()
		}()
		for {
		  rate.Add(1)
		  time.Sleep(10*time.Millisecond)
		}
	}

