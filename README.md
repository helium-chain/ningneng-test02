- 添加代码如下。
  - 本文使用了singleflight包，该包来自Google官方库，由于部分逻辑与当前不符合做了部分修改。
  - 参看 slf/slf.go 目录文件
```go
var (
    group = new(slf.Group)
)

func target(id string, job func()) (count int) {
	//TODO implement this

	res := group.DoChan(id, func() (interface{}, error) {
		// 执行job
		if job != nil {
			job()
		}

		// 没有需要返回的数据
		return nil, nil
	})

	result := <-res

	if result.Err != nil {
		log.Fatalf("singleflight错误：%v", result.Err)
	}

	fmt.Printf("id: %s, xx: %v\n", id, result.Count)

	return result.Count
}
```