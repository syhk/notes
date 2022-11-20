# go学习

```go
package main

import (
  "fmt"
  "time"
)

// 基础测试
func test1() {

  // switch 语句
  var grade string = "B"
  var marks int = 90

  switch marks {
  case 90:
    grade = "A"
  case 80:
    grade = "B"
  case 50, 60, 70:
    grade = "C"
  default:
    grade = "D"
  }
  fmt.Println(grade)

  /*
     select 语句类似于 switch 语句，但是 select 会随机执行一个可运行的 case
     如果没有 case 运行，它会阻塞，直到有  case 可运行
  */
  var c1, c2, c3 chan int
  var i1, i2 int
  select {
  case i1 = <-c1:
    fmt.Println("received", i1, "from c1\n")
  case c2 <- i2:
    fmt.Println("sent", i2, "to c2\n")
  case i3, ok := (<-c3):
    if ok {
      fmt.Println("received", i3, "fomr c3\n")
    } else {
      fmt.Println("c3 is closed\n")
    }
  default:
    fmt.Println("no communication\n", i1, i2, c1, c2, c3)
  }

}

func main() {
  // test1()

  /*
    操作系统线程和 goroutine 的关系：
    1. 一个操作系统线程对应用户态多个 goroutine.
    2. go程序可以同时使用多个操作系统线程
    3. goroutine 和 OS 线程是多对多的关系，m:n

  */
  go func() {
    i := 0
    for {
      i++
      fmt.Printf("new goroutine:i=%d\n", i)
      time.Sleep(time.Second)
    }
  }()

  i := 0
  for {
    i++
    fmt.Println(i)
    time.Sleep(time.Second)
    if i == 2 {
      break
    }

  }

}
```

关闭后的 chan 有以下特点：

- 
对一个关闭的通道发送值就会导致 panic

- 
对一个关闭的通道进行接收会一直获取值直到通道为空

- 
对一个关闭的并且没有值的通道执行接收操作会得到对应类型的零值

- 
关闭一个已经关闭的通道会导致 panic


![](image/image_YR-S97_4oZ.png#alt=)

```go
package main

import (
  "fmt"
)

func test2() {

  // 通道是引用类型，通道类型的空值是 nil
  // var ch chan int
  // 声明后的通道需要使用 make 函数初始化后才能使用
  // 通道的缓冲大小是可选的
  ch := make(chan int)
  ch <- 100 //把 100 发送到 ch 中
  fmt.Println(ch)
  x := <-ch //从 ch 中接收值并赋值给变量 x
  //<-ch      // 从 ch 中接收值，忽略结果
  fmt.Println(x)
  close(ch) //关闭通道；通道是可以被垃圾回收机制回收的

}

func recv(c chan int) {
  fmt.Println("接收成功", c)
  ret := <-c
  fmt.Println("接收成功", ret)
}
func test3() {
  ch1 := make(chan int)
  ch2 := make(chan int)
  go func() {
    for i := 0; i < 100; i++ {
      ch1 <- i
    }
    close(ch1)
  }()

  go func() {
    for {
      i, ok := <-ch1 //通道关闭后再取值 ok=false
      if !ok {
        break
      }
      ch2 <- i * i //将从 ch1 发送来的值的平方给发 ch2
    }
    close(ch2)
  }()

  for i := range ch2 { //通道关闭后会退出 for range 循环
    fmt.Println(i)
  }

  /*
     总结：有两种方式在接收值的时候判断通道是否关闭
     取值的时候会有一个返回值，通道是否关闭
     for range 循环在通道后会自动退出循环
  */
}

//单向通道:限制通道在函数中只能发送或者接收
func counter(out chan<- int) {
  for i := 0; i < 100; i++ {
    out <- i
  }
  close(out)
}

//chan<- int　是一个只能发送的通道，可以发送但是不能接收
//<-chan int 是一个只能接收的通道，可以接收但是不能发送
//在函数传递参数或赋值操作中可以将双向通道转换为单向通道，但是反过来是不可以的
func squarer(out chan<- int, in <-chan int) {
  for i := range in {
    out <- i * i
  }
  close(out)
}

func printer(in <-chan int) {
  for i := range in {
    fmt.Println(i)
  }
}
func test4() {
  ch1 := make(chan int)
  ch2 := make(chan int)
  go counter(ch1)
  go squarer(ch2, ch1)
  printer(ch2)
}

func main() {
  //test2()
  test3()
  //无缓冲通道的发送操作会阻塞，直到有接收操作。如果接收操作先执行，接收方将阻塞，直到有发送操作
  //ch := make(chan int)
  //go recv(ch)
  //ch <- 10
  //fmt.Println("发送成功")

  //  有缓冲通道
  //ch := make(chan int, 1) //创建一个容量为1 的有缓冲区通道，必须是 int 类型的值
  //fmt.Println(cap(ch))    //查看容量
  ////只要容量大于0就是有缓冲的；如果满了只能等待接收后现发送
  //ch <- 10
  //
  //close(ch)//通道 close 函数关闭 channel ，不需要通道的时候记得关闭通道
  //fmt.Println("发送成功")

}
```

### Golang init 函数执行顺序：

```
  init 函数 和 main 函数
  init() 函数：
  init 函数是用于程序执行前做包的初始化的函数
  每个包可以拥有多个 init 函数
  包的每个源文件也可以拥有多个　init 函数
  同一个包中多个 init 函数的执行顺序go
  不同包的init函数按照包导入的依赖关系决定该初始化函数的执行顺序
  init 函数不能被其他函数调用，而是在 main 函数执行之前，自动被调用

  相同点：
  两个函数在定义时不能有任何的参数和返回值，且Go程序自动调用
  不同点：
  init 可以应用任意包中，且可以重复定义多个
  main 函数只能用于 main 包中，且只能定义一个
```

![](image/image_dPsnxvTX9q.png#alt=)

import→const→var→init→main()

### 下划线 _

下划线是特殊标识符，用来忽略结果 

`import` 用于导入其他的 package

当导入一个包的时候仅仅希望执行 init() 函数而已，可以使用 `import _ 包路径` ，它只是引用该包而已，只是为了调用 init() 函数

```go
f,_ := os.Open("xxx")  _ 代表忽略返回的 error 变量
```

也可以用做占位符，如果方法返回两个值，只需要一个结果，就可以使用 _ 进行占位

```go
package main

import (
  "fmt"
  "sync"
  "time"
)

func test2() {

  // 通道是引用类型，通道类型的空值是 nil
  // var ch chan int
  // 声明后的通道需要使用 make 函数初始化后才能使用
  // 通道的缓冲大小是可选的
  ch := make(chan int)
  ch <- 100 //把 100 发送到 ch 中
  fmt.Println(ch)
  x := <-ch //从 ch 中接收值并赋值给变量 x
  //<-ch      // 从 ch 中接收值，忽略结果
  fmt.Println(x)
  close(ch) //关闭通道；通道是可以被垃圾回收机制回收的

}

func recv(c chan int) {
  fmt.Println("接收成功", c)
  ret := <-c
  fmt.Println("接收成功", ret)
}
func test3() {
  ch1 := make(chan int)
  ch2 := make(chan int)
  go func() {
    for i := 0; i < 100; i++ {
      ch1 <- i
    }
    close(ch1)
  }()

  go func() {
    for {
      i, ok := <-ch1 //通道关闭后再取值 ok=false
      if !ok {
        break
      }
      ch2 <- i * i //将从 ch1 发送来的值的平方给发 ch2
    }
    close(ch2)
  }()

  for i := range ch2 { //通道关闭后会退出 for range 循环
    fmt.Println(i)
  }

  /*
     总结：有两种方式在接收值的时候判断通道是否关闭
  */
}

//单向通道:限制通道在函数中只能发送或者接收
func counter(out chan<- int) {
  for i := 0; i < 100; i++ {
    out <- i
  }
  close(out)
}

//chan<- int　是一个只能发送的通道，可以发送但是不能接收
//<-chan int 是一个只能接收的通道，可以接收但是不能发送
//在函数传递参数或赋值操作中可以将双向通道转换为单向通道，但是反过来是不可以的
func squarer(out chan<- int, in <-chan int) {
  for i := range in {
    out <- i * i
  }
  close(out)
}

func printer(in <-chan int) {
  for i := range in {
    fmt.Println(i)
  }
}
func test4() {
  ch1 := make(chan int)
  ch2 := make(chan int)
  go counter(ch1)
  go squarer(ch2, ch1)
  printer(ch2)
}

//互斥锁
var x int64
var wg sync.WaitGroup
var lock sync.Mutex

func add() {
  for i := 0; i < 5000; i++ {
    lock.Lock()
    x = x + 1
    lock.Unlock()
  }
  wg.Done()
}

func test5() {
  wg.Add(2)
  go add()
  go add()
  wg.Wait()
  fmt.Println(x)
}

//读写互斥锁
//读写锁分为两种：读锁和写锁，
//当一个goroutine获取读锁之后，其他的goroutine如果是获取读锁会继续获得锁，如果是获取写锁就会等待；当一个goroutine获取写锁之后，其他的goroutine无论是获取读锁还是写锁都会等待。
//适合读多写少的场景
var (
  x1     int64
  wg1    sync.WaitGroup
  lock1  sync.Mutex
  rwlock sync.RWMutex
)

func write() {
  rwlock.Lock() //加写锁
  x1 = x1 + 1
  fmt.Println("写", x1)
  time.Sleep(10 * time.Millisecond) //10毫秒
  rwlock.Unlock()                   //解写锁
  wg.Done()
}
func read() {
  rwlock.RLock() //加读锁
  fmt.Println("read:", x1)
  time.Sleep(time.Millisecond) //1毫秒
  rwlock.RUnlock()             //解读锁
  wg.Done()
}
func test6() {
  start := time.Now()
  for i := 0; i < 10; i++ {
    wg.Add(1)
    go write()
  }
  for i := 0; i < 1000; i++ {
    wg.Add(1)
    go read()
  }

  wg.Wait()
  end := time.Now()
  fmt.Println(end.Sub(start))
  /*
     Sync.WaitGroup的方法：
     - add(int)  计数器+要启动的goroutine数
     - Done() 计数器 -1
     - Wait() 阻塞直到计数器变为 0
  */
  //Sync.Once 确保在高并发的场景下只执行一次
  //它只有一个方法 Do(f func())
  var Once sync.Once
  var s = [5]int{1, 3, 3, 4, 4}
  for range s {
    fmt.Println("外Once============")
    Once.Do(func() {
      fmt.Println("内===================") //保证只执行一次
    })
  }
}

//test7 基础
func test7() {
  /*
        值类型：
                bool
                  int(32 or 64), int8, int16, int32, int64
                  uint(32 or 64), uint8(byte), uint16, uint32, uint64
                  float32, float64
                  string
                  complex64, complex128
                  array    -- 固定长度的数组
        引用类型：
            slice   -- 序列数组(最常用)
            map     -- 映射
            chan    -- 管道

        内置函数：
            append          -- 用来追加元素到数组、slice中,返回修改后的数组、slice
          close           -- 主要用来关闭channel
          delete            -- 从map中删除key对应的value
          panic            -- 停止常规的goroutine  （panic和recover：用来做错误处理）
          recover         -- 允许程序定义goroutine的panic动作
          real            -- 返回complex的实部   （complex、real imag：用于创建和操作复数）
          imag            -- 返回complex的虚部
          make            -- 用来分配内存，返回Type本身(只能应用于slice, map, channel)
          new                -- 用来分配内存，主要用来分配值类型，比如int、struct。返回指向Type的指针
          cap                -- capacity是容量的意思，用于返回某个类型的最大容量（只能用于切片和 map）
          copy            -- 用于复制和连接slice，返回复制的数目
          len                -- 来求长度，比如string、array、slice、map、channel ，返回长度
          print、println     -- 底层打印函数，在部署环境中建议使用 fmt 包

  */

  /* init 函数 和 main 函数
  init() 函数：
  init 函数是用于程序执行前做包的初始化的函数
  每个包可以拥有多个 init 函数
  包的每个源文件也可以拥有多个　init 函数
  同一个包中多个 init 函数的执行顺序go
  不同包的init函数按照包导入的依赖关系决定该初始化函数的执行顺序
  init 函数不能被其他函数调用，而是在 main 函数执行之前，自动被调用

  相同点：
  两个函数在定义时不能有任何的参数和返回值，且Go程序自动调用
  不同点：
  init 可以应用任意包中，且可以重复定义多个
  main 函数只能用于 main 包中，且只能定义一个

  类型转换：
  T(表达式)
  */

  //  数组定义
  //var arr1 [5]int = [5]int{1, 2, 3}
  //var arr2 = [5]int{2, 2, 2}
  //var str = [5]string{3: "syhk", 4: "tom"} //可以指定位置
  ////  通过初始化值确定长度
  //arr3 := [...]int{3, 3, 3, 3}
  //arr4 := [...]struct {
  //  name string
  //  age  int
  //}{
  //  {"user1", 10},
  //  {"user2", 20}, //最后一行也需要加逗号
  //}
  //// 多维数组
  //arr5 := [...][3]int{{2, 2}, {3, 3}, {4, 4}}
  //
  ////  切片
  ////声明切片
  //var s1 []int
  //var s2 []int = make([]int, 0)
  //var s3 []int = make([]int, 0, 0) //初始化并赋值
  //s4 := arr3[1:3]

  //new 和 make 的区别:
  //make 只用于 slice map  channel 的初始化，返回的还是这三个引用类型本身
  //new 用于类型的内存分配，并且内存对应的值为类型零值，返回的是指向类型的指针

  //  go 语言的指针
  var z int = 10
  ptr := &z
  fmt.Println(*ptr) //值
  fmt.Println(&ptr) //指针的地址
  fmt.Println(ptr)  //z的地址
  var ns *string
  fmt.Println(ns) //空指针值为 nil

  //  map
  var map1 map[string]int
  //map 需要使用 make 来分配内存；使用前不分配内存，会报错 panic
  map1 = make(map[string]int, 8)
  map1["syhk1"] = 90
  map1["syhk2"] = 100
  fmt.Println(map1)
  fmt.Println(map1["syhk1"])
  va, ok := map1["syhk1"] //判断是否存在这个键
  fmt.Println(ok, va)

  //  遍历 mpa
  for k, v := range map1 {
    fmt.Println(k)
    fmt.Println(v)
  }
  //  只遍历 key
  for k := range map1 {
    fmt.Println(k)
  }
  delete(map1, "syhk1") //删除一组键值对

}
func init() {
  fmt.Println("2号 init()")
}

func init() {
  fmt.Println("1号 init()")
}

//根据定义顺序执行，一个文件有多个 init 函数

func main() {
  //test2()
  //test3()
  //test4()
  //test5()
  //test6()
  test7()
  //无缓冲通道的发送操作会阻塞，直到有接收操作。如果接收操作先执行，接收方将阻塞，直到有发送操作
  //ch := make(chan int)
  //go recv(ch)
  //ch <- 10
  //fmt.Println("发送成功")

  //  有缓冲通道
  //ch := make(chan int, 1) //创建一个容量为1 的有缓冲区通道，必须是 int 类型的值
  //fmt.Println(cap(ch))    //查看容量
  ////只要容量大于0就是有缓冲的；如果满了只能等待接收后现发送
  //ch <- 10
  //
  //close(ch)//通道 close 函数关闭 channel ，不需要通道的时候记得关闭通道
  //fmt.Println("发送成功")

}
```

### atomic 原子包

```go
package main

import (
  "fmt"
  "sync"
  "sync/atomic"
  "time"
)

//原子操作 atomic 包
var x4 int64
var l4 sync.Mutex
var wg4 sync.WaitGroup

//普通版加函数
func add4() {
  x4++
  wg4.Done()
}

//互斥锁版
func mutexAdd() {
  l4.Lock()
  x4++
  l4.Unlock()
  wg4.Done()
}

//原子版
func atomicAdd() {
  atomic.AddInt64(&x4, 1)
  wg4.Done()
}

func main() {

  start4 := time.Now()
  for i := 0; i < 100000; i++ {
    wg4.Add(1)
    //go add4()
    //99964 每次结果都不一样，并发不安全
    //25.404ms

    //go mutexAdd()
    //100000
    //28.0398ms

    go atomicAdd()
    //  100000
    //36.6098ms
  }
  wg4.Wait()
  end4 := time.Now()
  fmt.Println(x4)
  fmt.Println(end4.Sub(start4))
}
```

### GMP 原理与调度

![](image/image_9Pq8LWCF9q.png#alt=)

![](image/image_3ZjoOhfNKl.png#alt=)

go func 的调度流程

![](image/image_xBW3MUk94J.png#alt=)
