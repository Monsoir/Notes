# 指针

## 操作符

Go 中可以使用指针，`*T` 表示指向 `T` 类型的指针，零值为 `nil`

```go
var p *int
```

---

`&` 生成一个指向操作数的类型的指针

```go
i := 42
p = &i
```

---

`*` 表示指向指针所指向位置的底层值

```go
fmt.Println(*p) // 通过指针 p 获取到指针 int 类型指针 p 所指向的 int 类型的值
*p = 21 // 将 21 赋值到指针 p 所指向的位置
```

> Go 中没有像 C 中一样的指针运算


