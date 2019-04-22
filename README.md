# Go Channels
A channel is a communication mechanism that lets one goroutine send values to another goroutine.

## Create a channel
To create a channel we have the `make` function:
```go
ch := make(chan int) // ch has type 'chan int'
````

## Unbuffered channel
[Unbuffered channels](./unbuffered/) have no capacity and therefore require both goroutines to be ready to make any exchange. 

## Buffered channel
[Buffered channels](./buffered/) have capacity, this means that we can send values into the channel (up to its capacity) without a corresponding concurrent receive.

## Close a channel
When we have finished writing into a channel we can close it. See [Closed channels](./closed-channels/). 

## nil channels
[nil channels](./nil-channels/) are very useful for disabling a case in a select statement.

## References
- `The Go Programming Language` by Brian Kernighan.

