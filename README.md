# Go Channels
A channel is a communication mechanism that lets one goroutine send values to another goroutine.

## Unbuffered channel
[Unbuffered channels](./unbuffered/) have no capacity and therefore require both goroutines to be ready to make any exchange. 

## Buffered channel
[Buffered channels](./buffered/) have capacity, this means that we can send values into the channel (up to its capacity) without a corresponding concurrent receive.

## Create a channel
To create a channel we have the `make` function.

### Creating an unbuffered channel
Creates an unbuffered channel of channel's type `int`:
```go
ch := make(chan int)
````

### Creating a buffered channel
Creates a buffered channel with capacity 2 of channel's type `int`:
```go
ch := make(chan int, 2) 
```

## Channel operations
A channel has three operations: `send`, `receive` and `close`. 

### Send
```go
ch <- x  // a send statement
```

### Receive
```go
x = <-ch // a receive expression in an assignment statement
<-ch     // a receive statement; result is discarded
```

### Close
The `close` operation sets a flag indicating that no more values will ever be sent on this channel; subsequent attempts to send will panic. Receive operations on a [closed channel](./closed-channels/) yield the values that have been sent until no more values are left; any receive operations there after complete immediately and yield the zero value of the channel’s element type.

```go
close(ch)
```
 

## nil channels
[nil channels](./nil-channels/) are very useful for disabling a case in a select statement.

## References
- `The Go Programming Language` by Brian Kernighan.

