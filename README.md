# Go Channels
A channel is a communication mechanism that lets one goroutine send values to another goroutine.

## Create a channel
To create a channel we have the `make` function:
```go
ch := make(chan int) // ch has type 'chan int'
````

## Close a channel
When we have finished writing into a channel we can close it. See [Closed channels](./closed-channels/). 

## nil channels
[nil channels](./nil-channel/) are very useful for disabling a case in a select statement.

## References
- `The Go Programming Language` by Brian Kernighan.
