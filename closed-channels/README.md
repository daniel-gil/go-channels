# Closed Channels

To close a channel we have the `close` function:
```go 
close(ch)
```
## Sending to closed channel
Once a channel has been closed, you cannot send a value on this channel, it will
panic:
```go
ch := make(chan int)
close(ch)

// Sending a value to a closed channel
ch <- 8
```
This is the panic message returned:
```bash
panic: send on closed channel
````

## Receiving from closed channel
A closed channel is not blocked for reading from it, we will receive the zero value 
for that channel's type infinitely. 

For example for a channel of type `int` we will receive 0, for `bool` false,
for string an empty string "", etc...

```go
ch := make(chan int)
close(ch)

// Reading from a closed channel
v := <-ch

fmt.Printf("value read from channel: %d",v)
```
Here is the output:
```bash
value read from channel: 0
```

## Check if channel is closed
### Open State of the Channel
To identify if a channel is already closed we can check the open state of the channel, 
it is a boolean, in the following example it corresponds to the variable `ok`:

```go
v, ok := <- ch
if !ok {
    fmt.Print("the channel is closed")
} else {
    fmt.Printf("the value received from channel is %v", v)
}
```

### Range over the Channel
As an alternative to check the open state of the channel we could range over the channel,
it will exit the loop once a channel has been drained.
```go
ch := make(chan bool, 2)
ch <- true
ch <- true
close(ch)

for v := range ch {
    fmt.Println(v) // called twice
}
```

it reads values from the channel until it's closed.

## Closing a closed channel
If we try to close an already closed channel it will panic:
```go
ch := make(chan int)
close(ch)

// here ch it's a closed channel

close(ch)
```

Here is the panic message received:
```bash
panic: close of closed channel
```

## References
- `The Go Programming Language` by Brian Kernighan.
- [Curious Channels](https://dave.cheney.net/2013/04/30/curious-channels) by Dave Cheney.
