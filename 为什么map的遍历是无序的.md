* map再遍历的时候并不是固定的从序号为0的bucket开始遍历，每次遍历，都会从一个随机序号的bucket开始，再从其中随机的cell开始遍历，也就是说map的遍历有两层随机
* 假设map遍历时，是按序遍历bucket，同时按需遍历bucket中和其overflow bucket中的cell。但是map在扩容后，会发生key的搬迁，这造成 原来落在一个bucket中的key,搬迁后，有可能会落到其他bucket中了，从这个角度看，遍历map的结果就不可能是按照原来的顺序了

map本身是无序的，且遍历时的顺序还会被随机化，如果想顺序遍历map，可以借助数组或者切片对map的key进行排序，再按照key的顺序遍历map
```
var arr0 []int //切片
var arr1 [1]int //数组

for key, _ := range map {
  arr0 = append(arr0, key)
}

sort.Ints(arr0) //排序

for _, v := range arr0 {
  fmt.Println( v, map[v] )
}
```
