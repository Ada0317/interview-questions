map是线程不安全的，他不支持并发读写
* 因为在go官方的眼里，Go map更多的适用于 经典的使用场景（不需要从多个gorountine并发进行安全访问），而不是为少部分并发访问情况，而导致大部分程序付出加锁的性能代价
如果想要线程安全的使用map 可以使用
* 方式一： map 与 sync.RWMutex
```
m := make(map[int]int)
var lock sync.RWMutes
for i:=0; i < 100; i++ {
  go func(i int){
    lock.Lock()
    m[i] = i
    lock.Unlock()
  }(i)
}
 
for i:=0; i<100; i++ {
  go func(i int){
    lock.RLock()
    fmt.Println(s[i])
    lock.RUnloc()
  }(i)
 }
  time.Sleep(1*time.Sencond)
```

* 方式二： sync.Map
```
func main() {
var m sync.Map
for i := 0; i < 100; i++ {
  go func(i int) {
    m.Store(i, i)
  }(i)
}
for i := 0; i < 100; i++ {
  go func(i int) {
    v, ok := m.Load(i)
    fmt.Printf("Load: %v, %v", v, ok)
  }(i)
}

time.Sleep(1 * time.Second)
}

```
