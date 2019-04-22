# nil Channels

## Sending to nil channel
nil channels blocks forever when trying to send to them:

```go
// Create an uninitialized (nil) channel.
var ch chan int

// Sending on a nil channel will block forever.
ch <- 5
```
## Receiving from nil channel
nil channels blocks forever when trying to receive from them:

```go
// Create an uninitialized (nil) channel.
var ch chan int

// Receiving on a nil channel blocks forever.
<-ch
```

## Closing a nil channel
Closing a nil channel will panic with the following panic message:

```go 
var c chan int
close(c)
```
```bash
panic: close of nil channel
```


## Usages

### Disable a case in a select statement

The following example has been extracted from episode #26 of `justforfunc` (see references).

The main program creates 2 channels, insert some values into them and finally close each channel.

The `merge` function combines the values from both input channels into a third one. 
The case-statement detects that the channel has been closed checking if the `ok` is`false`.
If we do nothing we will receive a burst of nil values from the closed channel and this
will impact the application performance. To avoid this situation we can set the closed
channel to nil and this will block forever, meaning we have disabled the case-statement.


```go 
func main() {
    a := asChan(1, 3, 5, 7)
    b := asChan(2, 4, 6, 8)
    c := merge(a, b)
    for v := range c {
        fmt.Println(v)
    }
}

func asChan(vs ...int) <-chan int {
    c := make(chan int)
    go func() {
        for _, v := range vs {
            c <- v
            time.Sleep(time.Duration(rand.Intn(1000)) * time.Millisecond)
        }
        close(c)
    }()
    return c
}

func merge(a, b <-chan int) <-chan int {
    c := make(chan int)
    go func() {
        defer close(c)
        for a != nil || b != nil {
            select {
            case v, ok := <-a:
                if !ok {
                    fmt.Println("a is done")
                    a = nil
                    continue
                }
                c <- v
            case v, ok := <-b:
                if !ok {
                    fmt.Println("b is done")
                    b = nil
                    continue
                }
                c <- v
            }
        }
    }()
    return c
}
```

## References
- [Why are there nil channels in Go?](https://medium.com/justforfunc/why-are-there-nil-channels-in-go-9877cc0b2308) by Francesc Campoy.
- [Nil Channels Always Block](https://www.godesignpatterns.com/2014/05/nil-channels-always-block.html)