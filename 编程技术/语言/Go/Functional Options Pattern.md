对于一个函数的配置方法，比如服务器的启动配置，一般有三种方式：

1. 为不同的参数定义不同的创建类（最差）
2. 定义一个`struct`来传递配置，也能获取可维护性
3. 使用 Functional Options Pattern 来实现，方式如下：

```go
package server

type Server {
  host string
  port int
  timeout time.Duration
  maxConn int
}

func New(options ...func(*Server)) *Server {
  svr := &Server{}
  for _, o := range options {
    o(svr)
  }
  return svr
}

func (s *Server) Start() error {
  // todo
}

func WithHost(host string) func(*Server) {
  return func(s *Server) {
    s.host = host
  }
}

func WithPort(port int) func(*Server) {
  return func(s *Server) {
    s.port = port
  }
}

func WithTimeout(timeout time.Duration) func(*Server) {
  return func(s *Server) {
    s.timeout = timeout
  }
}

func WithMaxConn(maxConn int) func(*Server) {
  return func(s *Server) {
    s.maxConn = maxConn
  }
}
```

ref: https://golang.cafe/blog/golang-functional-options-pattern.html