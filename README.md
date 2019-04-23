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
ch := make(chan int)    // unbuffered channel
ch := make(chan int, 0) // unbuffered channel
````

### Creating a buffered channel
Creates a buffered channel with capacity 2 of channel's type `int`:
```go
ch := make(chan int, 2)  // buffered channel with capacity 2
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
If the sender knows that no further values will ever be sent on a channel, it is useful to communicate this fact to the receiver goroutines so that they can stop waiting. This is accomplished by closing  he channel using the built-in close function:

```go
close(ch)
```

The `close` operation sets a flag indicating that no more values will ever be sent on this channel; subsequent attempts to send will panic. Receive operations on a [closed channel](./closed-channels/) yield the values that have been sent until no more values are left; any receive operations there after complete immediately and yield the zero value of the channel’s element type.

You needn’t close every channel when you’ve finished with it. It’s only necessary 
to close a channel when it is important to tell the receiving goroutines that all data have been sent.


## Unidirectional Channel Types
An unidirectional channel type defines a channel for `send-only` operation:
```go
ch := make(chan<- int)
```

or for `receive-only` operation:
```go
ch := make(<-chan int)
```

The `close` operation it's only allowed by the sending goroutine, so we have a compile-time error when attempting to close a receive-only channel:
```
invalid operation: close(ch) (cannot close receive-only channel)go
```

## nil channels
[nil channels](./nil-channels/) are very useful for disabling a case in a select statement.

## References
- `The Go Programming Language` by Brian Kernighan.
