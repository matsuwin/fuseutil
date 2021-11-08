# siggroup
基于系统信号量的异步任务并行管理组

<br>

## Quick Start

```go
// 添加异步任务 work_1，后台持续运行。
siggroup.Async(func() (_ error) {
    fmt.Println("work_1 ...")
    for {}
})

// 添加异步任务 work_2，短暂运行后退出。
siggroup.Async(func() (_ error) {
    fmt.Println("work_2 ...")
    time.Sleep(time.Second)
    return
})

// 等待任务结束，注意！只要有一个任务退出就退出所有。
siggroup.Wait(func() {
    fmt.Println(":shutdown")
})
```
```
work_2 ...
work_1 ...
:shutdown
```

## Installing

```
go get github.com/matsuwin/siggroup
```

<br>

# X

**errcause**

```go
package main

import (
    "github.com/matsuwin/siggroup/x/errcause"
    "github.com/pkg/errors"
    "io/ioutil"
)

func mkError() (_ error) {
    _, err := ioutil.ReadFile("xxx.txt")
    if err != nil {
        return errors.New(err.Error())
    }
    return
}

func main() {

    // 错误恢复 recover call errcause.Keep
    defer errcause.Recover()

    // 模拟一个错误抛出调用
    if err := mkError(); err != nil {
        panic(err)
    }
}
```
