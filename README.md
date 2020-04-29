# accumulator

This is a fast, thread safe accumulator. The implementation is based on a cyclic buffer. 
Application calls Add() to add a value to the accumulator. Application calls Tick() to move to the next entry.

	import (
		"github.com/larytet-go/accumulator"
	)

	func main() {
	        // Sliding window with 32 items 
		rate := accumulator.New("rate", 32)
		go func() {
		  // Move the sliding window by one every second
		  time.Sleep(1*time.Second)
		  rate.Tick()
		}()
		
		for {
		  // I expect the counters to be in the 99-101 range  
		  rate.Add(1)
		  time.Sleep(10*time.Millisecond)
		}
	}

