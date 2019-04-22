
# Unbuffered channel
Unbuffered channels have no capacity and therefore require both goroutines to be ready to make any exchange. When a goroutine attempts to send a resource to an unbuffered channel and there is no goroutine waiting to receive the resource, the channel will lock the sending goroutine and make it wait. When a goroutine attempts to receive from an unbuffered channel, and there is no goroutine waiting to send a resource, the channel will lock the receiving goroutine and make it wait.

## Example

```go

func main() {
    // Create a new channel with `make(chan val-type)`.
    // Channels are typed by the values they convey.
    messages := make(chan string)

    // _Send_ a value into a channel using the `channel <-`
    // syntax. Here we send `"ping"`  to the `messages`
    // channel we made above, from a new goroutine.
    go func() { messages <- "ping" }()

    // The `<-channel` syntax _receives_ a value from the
    // channel. Here we'll receive the `"ping"` message
    // we sent above and print it out.
    msg := <-messages
    fmt.Println(msg)
}
```

Output:
```
ping
```

## References
- [Go by Example: Channels](https://gobyexample.com/channels)
- [The Nature Of Channels In Go](https://www.ardanlabs.com/blog/2014/02/the-nature-of-channels-in-go.html) by 
William Kennedy.