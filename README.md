# 函数

## 1. 函数声明

```go
func 函数名(参数列表) (返回值列表){
  函数体
}

/**
* 函数名：add
* a，b: 类型为int64的形参, 若参数类型相同，则可以只写一个类型
* int64：返回值类型
*/
func add(a, b int64) int64{
  return a + b;
}

fmt.Prinltn(add(1,2)) // 输出 3
```

函数签名的返回值列表处，可以说包含返回值名称，函数结尾只需（必须）以return语句返回

```go
func add(a, b int64) (c int64){
  c = a + b;
  return
}

fmt.Prinltn(add(1,2)) // 输出 3
```

## 2. 多返回值

在Go语言中，一个函数可以有多个返回值。标准款中许多函数有两个返回值，第一个是期望得到的有意义的返回值，第二个则是函数出错时的错误信息。

```go
func open(file File) ([]string, error)
func HourMinSec(t time.Time) (hour, minute, second int) 
```

接收返回值

```go
// 显示分配给不同变量
ans, err := open(file)
// 如果有返回值不需要被接收，则分配给下划线_
hour,miniute, _ := HourMinsec(22:30:00)
```

## 3. 错误

Go中常给函数定义一个额外的布尔型返回值，命名为ok，代表程序是否运行成功。

```go
// 例如从缓存中查找一个不存在的key
value, ok = cache.get(key) 
if !ok {
  // key not exist
}
```

ok只能反映函数是否成功，当需要更多的错误信息，则应该返回error类型。如果调用成功了，则error值为nil，否则打印出错误信息

```go
// 如果http的Get请求失败，则将错误信息返回给调用者
response, err := http.Get(url) 
if err != nil {
  fmt.Println(err);
}
```

error往往是能预知的错误。例如数组越界等不可预知的错误，会导致程序异常退出，在Go中称之为Panic。

## 4. Deferred函数

在Python、Java中对于异常，有try...catch...机制。Go语言中类似的机制为defer和recover。

defer常用于打开、关闭、连接、断开连接、加锁、释放锁。

```go
var mu sync.Mutex
var m = make(map[string]int)
func lookup(key string) int {
    mu.Lock()
    defer mu.Unlock()  
    return m[key]
}
```

## Panic 异常

Go的类型系统会在编译时捕获很多错误，但有些错误只能在运行时检查，如数组访问越界、空指针引用等。这些运行时错误会引起painc异常。
