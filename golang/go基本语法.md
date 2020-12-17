基本语法和简单常识

```go
// 交换函数
// 输入两个指针
func test(num int)  {
   // 获取数据的地址
   ptr := &num
   fmt.Println(ptr)
   // 获取指针所指向的地址的值
   temp := *ptr
   fmt.Println(temp)
   // 通过指针对数据进行赋值
   *ptr = 200
   fmt.Println(num)
}
func swap(a , b *int )   {
   temp := *a
   *a = *b
   *b = temp
}
```

堆（stack）

go语言默认将函数局部变量分配到栈上，函数执行完毕，局部变量就会被回收，整个过程非常快

堆在内存中分配，对更适合不可预知大小的内存分配，代价是分配速度比较慢，而且还会产生内存碎片

变量逃逸是指GO语言编译器将数据分配给栈还是堆的考虑过程



```go
tip1 := "genji is a ninjia"
fmt.Println(len(tip1))

tip2 := "忍者"
//会输出ASCll码的长度，每个中文占三个字符的长度
fmt.Println(len(tip2))
// 使用utf8 地下的这个方法，就会输出正确的方法
fmt.Println(utf8.RuneCountInString(tip2))
tip3 := "吹NB"
fmt.Println(len(tip3))
fmt.Println(utf8.RuneCountInString(tip3))
```



字符串遍历

```go
str := "小母牛拿着菜刀砍电线，一路火花带闪电"
// 这样遍历的结果是每个数组的ASCll码 对应的值,这是采用ASCLL进行遍历的
for i := 0; i < len(str); i++ {
   fmt.Printf("ASCLL : %c %d\n",str[i],str[i])
}
```

```go
// 字符串的遍历
// 和字符数组一样
str := "小母牛拿着菜刀砍电线，一路火花带闪电"
// 这样是采用Unicode码进行遍历
for _, s :=range str{
   fmt.Printf("Unicode : %c %d\n",s,s)
}
```