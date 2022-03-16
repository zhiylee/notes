Go 语言标准库 `testing` 内置了 benchmark，进行性能测试时，尽可能保持测试环境的稳定。

## 编写用例

```go
// fib_test.go
package main

import "testing"

func BenchmarkFib(b *testing.B) {
	for n := 0; n < b.N; n++ {
		fib(30) // run fib(30) b.N times
	}
}
```

- benchmark 用例和普通的单元测试用例一样，都位于 `*_test.go` 文件中。
- 函数名以 `Benchmark` 开头，参数是 `b *testing.B`。
- `b.ResetTimer()` 可重置定时器
-  `b.StopTimer()` 暂停计时
-  `b.StartTimer()` 开始计时

## 运行用例

```shell
go test -bench <module name>/<package name>
```



> `go test <module name>/<package name>` 用来运行某个 package 内的所有测试用例。
>
> - 运行当前 package 内的用例：`go test <package name>` 或 `go test .`
> - 运行子 package 内的用例： `go test <module name>/<package name>` 或 `go test ./<package name>`
> - 如果想递归测试当前目录下的所有的 package：`go test ./...` 或 `go test <module name>/...`

`go test` 命令默认不运行 benchmark 用例的，如果我们想运行 benchmark 用例，则需要加上 `-bench` 参数

```shell
$ go test -bench .
goos: darwin
goarch: amd64
pkg: example
BenchmarkFib-8               200           5865240 ns/op
PASS
ok      example 1.782s
```

`-bench` 参数支持传入一个正则表达式，匹配需要执行的用例，如`go test -bench='Fib$'`。

我们仔细观察上述例子的输出：

```shell
BenchmarkFib-8               202           5980669 ns/op
```

BenchmarkFib-8 中的 `-8` 即 `GOMAXPROCS`，默认等于 CPU 核数，`202` 和 `5980669 ns/op` 表示用例执行了 202 次，每次花费约 0.006s。

### -cpu

可以通过 `-cpu` 参数改变 `GOMAXPROCS`，`-cpu` 支持传入一个列表作为参数，例如：

```shell
$ go test -bench='Fib$' -cpu=2,4 .
goos: darwin
goarch: amd64
pkg: example
BenchmarkFib-2               206           5774888 ns/op
BenchmarkFib-4               205           5799426 ns/op
PASS
ok      example 3.563s
```

在这个例子中，改变 CPU 的核数对结果几乎没有影响，因为这个 Fib 的调用是串行的。

### -benchtime

benchmark 的默认时间是 1s

 `-benchtime=5s` 指定为 5s

 `-benchtime=30x`执行30次

### -count

用来设置 benchmark 的轮数

`-count=3`重复测3次



## benchmark 是如何工作的

benchmark 用例的参数 `b *testing.B`，有个属性 `b.N` 表示这个用例需要运行的次数。`b.N` 对于每个用例都是不一样的。

那这个值是如何决定的呢？`b.N` 从 1 开始，如果该用例能够在 1s 内完成，`b.N` 的值便会增加，再次执行。`b.N` 的值大概以 1, 2, 3, 5, 10, 20, 30, 50, 100 这样的序列递增，越到后面，增加得越快。



https://geektutu.com/post/hpg-benchmark.html
