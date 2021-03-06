# Buffered Channels
Buffered channels, also called synchronous channels, have capacity and therefore can behave a bit differently. When a goroutine attempts to send a resource to a buffered channel and the channel is full, the channel will lock the goroutine and make it wait until a buffer becomes available. If there is room in the channel, the send can take place immediately and the goroutine can move on. When a goroutine attempts to receive from a buffered channel and the buffered channel is empty, the channel will lock the goroutine and make it wait until a resource has been sent.

## Usage

```go
func main() {

    // Here we `make` a channel of strings buffering up to
    // 2 values.
    messages := make(chan string, 2)

    // Because this channel is buffered, we can send these
    // values into the channel without a corresponding
    // concurrent receive.
    messages <- "buffered"
    messages <- "channel"

    // Later we can receive these two values as usual.
    fmt.Println(<-messages)
    fmt.Println(<-messages)
}
```
Output 
```bash
buffered
channel
```

## Deadlock

```go

func main() {

    // Here we `make` a channel of strings buffering up to 2 values.
    messages := make(chan string, 2)

    // Because this channel is buffered, we can send these
    // values into the channel without a corresponding
    // concurrent receive.
    messages <- "buffered"
    messages <- "channel"
    messages <- "deadlock" // <-- this message is blocked forever pending to be written into the channel
}
```

Output 
```bash
fatal error: all goroutines are asleep - deadlock!
```

## Capacity
The capacity of a channel indicates how many elements is capable to hold the channel 
before the sending operation blocks. The function `cap` returns the capacity of a channel:
```go
ch := make(chan int, 10)
fmt.Println(cap(ch)) // "10"
```

## Length
The function `len` returns the number of elements inside the channel:
```go
ch := make(chan int, 10)
ch <- 10
ch <- 20
ch <- 30
fmt.Println(len(ch)) // "3" 
```

## References
- [Go by Example: Channels](https://gobyexample.com/channels)
- [The Nature Of Channels In Go](https://www.ardanlabs.com/blog/2014/02/the-nature-of-channels-in-go.html) by 
William Kennedy.