## Golang 线程同步

通常在 Go 语言中有两种方法可以用来做线程同步

1. sync.Cond
2. channel

channel 的很好理解，当我们从一个 channel 中接收数据的时候，如果里面没有数据，那我们直接就阻塞在那里了；在这篇文章中就来看看怎么使用 sync.Cond 完成同步



## sync.Cond

**定义结构体和方法**

```go
type BlueberryInt struct {
	num  int
	lock sync.Mutex
	cond *sync.Cond
}

func NewBlueberryInt() *BlueberryInt {
    // 创建 BlueberryInt 结构体
	var obj BlueberryInt
	obj.num = 0
	obj.cond = sync.NewCond(&obj.lock) // 使用结构体中的 lock 作为锁
	return &obj // 返回该结构体的指针
}

func (b *BlueberryInt) IncreaseLocked() {
    // IncreaseLocked 意味着在做加法操作的时候这个函数需要上锁后才能使用
	b.num++
}

func (b *BlueberryInt) DecreaseLocked() {
    // IncreaseLocked 意味着在做减法操作的时候这个函数需要上锁后才能使用
	b.num--
}

func (b *BlueberryInt) Signal() {
    // 当完成一件事情后，我们就发送 Signal
	b.cond.Signal()
}

func (b *BlueberryInt) Wait() {
    // 当我们调用 Wait 的时候，我们还不能马上执行操作
    // 我们需要收到 Signal 后 才可以继续执行
	b.cond.Wait()
}

func (b *BlueberryInt) Lock() {
    // 上锁
	b.lock.Lock()
}

func (b *BlueberryInt) UnLock() {
    // 解锁
	b.lock.Unlock()
}
```



**主要逻辑**

我们分别做 1000 次加法和 1000 次减法，但是呢，我们先让做减法的 goroutine 运行起来，并且使用 `Sleep` 等待 1 秒钟，确保它已经跑起来了；如果我们没有使用 `sync.Cond` 的话，这时候减法操作拿到锁之后就可以直接完成 1000 次的减法了，但是我们调用了 `num.Wait()` 来等待信号

为什么我们在做减法的 goroutine 中已经使用了 `num.Lock` 之后，在做加法的 goroutine 中还能够获得锁呢？这就是因为我们在 `NewBlueberryInt` 中的这行代码 `obj.cond = sync.NewCond(&obj.lock)` ，但我们调用 `Wait` 的时候，它会先释放我们传入的那把锁并且阻塞在那里，然后等待信号的到来，当它收到信号之后重新获取那把锁然后再继续执行操作

```go
func main() {
	// 做了加法才能做减法
	b := NewBlueberryInt()
	fmt.Printf("the num b is %d \n", b.num)

	// 做减法 1000 次
	go func(num *BlueberryInt) {
		num.Lock()
		num.Wait() // 等待信号
		for i := 0; i < 1000; i++ {
			num.DecreaseLocked()
		}
		num.UnLock()
	}(b)

	time.Sleep(time.Second)

	// 做加法 1000 次
	go func(num *BlueberryInt) {
		num.Lock()
		for i := 0; i < 1000; i++ {
			num.IncreaseLocked()
		}
		num.Signal() // 发送信号
		num.UnLock() // 一定要记得释放锁，不然做减法的 goroutine 那里就永远走不动了
	}(b)

	time.Sleep(time.Second)

	b.Lock()
	fmt.Printf("the num b is %d \n", b.num)
	b.UnLock()
}
```

最后的输出结果就是这样的：

```
the num b is 0 
the num b is 0
```

