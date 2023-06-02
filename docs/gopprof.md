# go 性能分析


## 环境要求





## 内存分析 memory profile


```sh
go tool pprof http://localhost:6060/debug/pprof/heap
```

输入 top 查看信息

```sh
webdev@XTZJ-20210127QV:~/webcloud/bin$ go tool pprof http://localhost:6060/debug/pprof/heap
Fetching profile over HTTP from http://localhost:6060/debug/pprof/heap
Saved profile in /home/webdev/pprof/pprof.webcloud.alloc_objects.alloc_space.inuse_objects.inuse_space.004.pb.gz
File: webcloud
Build ID: 9d3bfac18139bed0250d203e80c1d81142562a75
Type: inuse_space
Time: May 6, 2023 at 2:31pm (CST)
Entering interactive mode (type "help" for commands, "o" for options)
(pprof) top
Showing nodes accounting for 8910.55kB, 100% of 8910.55kB total
Showing top 10 nodes out of 38
      flat  flat%   sum%        cum   cum%
 3087.53kB 34.65% 34.65%  6167.17kB 69.21%  github.com/gorilla/websocket.newConn
 3079.63kB 34.56% 69.21%  3079.63kB 34.56%  bufio.NewReaderSize (inline)
 1068.05kB 11.99% 81.20%  1068.05kB 11.99%  wapcloud/pkg/robusta/binary/proto.init
  650.62kB  7.30% 88.50%   650.62kB  7.30%  bufio.NewWriterSize
  512.62kB  5.75% 94.25%  1024.71kB 11.50%  google.golang.org/protobuf/internal/filedesc.(*Message).unmarshalFull
  512.09kB  5.75%   100%   512.09kB  5.75%  google.golang.org/protobuf/internal/filedesc.(*Enum).unmarshalFull
         0     0%   100%  6167.17kB 69.21%  github.com/gorilla/websocket.(*Dialer).Dial
         0     0%   100%  6167.17kB 69.21%  github.com/gorilla/websocket.(*Dialer).DialContext
         0     0%   100%  1024.71kB 11.50%  google.golang.org/protobuf/internal/filedesc.(*File).lazyInit
         0     0%   100%  1024.71kB 11.50%  google.golang.org/protobuf/internal/filedesc.(*File).lazyInitOnce
(pprof) 
```




web 可视化


go tool pprof -http=":8080" http://localhost:6060/debug/pprof/profile


显示测试
hey -n 1000 http://localhost:6060/pprof-test


go tool pprof http://localhost:6060/debug/pprof/profile?seconds=120

go tool pprof -http=":8080" http://localhost:6060/debug/pprof/profile