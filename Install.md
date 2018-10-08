
### 一、安装
##### 1. 安装protoc ( Protobuf 编译器 )
```
下载地址

https://github.com/protocolbuffers/protobuf/releases/
放到环境变量的目录中
```

##### 2. 获取 goprotobuf 提供的 Protobuf 插件 protoc-gen-go
```
go get github.com/golang/protobuf/protoc-gen-go
go build
go install
```
##### 3. 获取 goprotobuf 提供的支持库，包含诸如编码（marshaling）、解码（unmarshaling）等功能
```
go get github.com/golang/protobuf/proto
go build
go install
```

2, 3 放到$GOPATH/bin目录中

---
### 二、使用
##### 1. 手动创建.proto文件
```
test.proto
  1 syntax="proto3";
  2 
  3 package tutorial;
  4 
  5 message Person {
  6         string  name  = 1;
  7         int32   id = 2;
  8         string  email = 3;
  9         enum PhoneType {
 10                 MOBILE  = 0;
 11                 HOME    = 1;
 12                 WORK    = 2;
 13         }
 14 
 15         message PhoneNumber {
 16                 string number = 1;
 17                 PhoneType type = 2;
 18         }
 19 
 20         repeated PhoneNumber phones = 4;
 21 }

```
##### 2. 利用protoc-gen-go插件生成*.proto.go文件
```
执行 protoc --go_out=. *.proto 生成 test.pb.go 文件
```
##### 3. 进行序列化和反序列化
```
package main

import (
	"fmt"
	im "tutorial"
	proto "github.com/golang/protobuf/proto"
)

func main() {
	var user *im.Person = &im.Person{
		Name:"meng",
		Id:100,
		Email:"123@126.com"
	}
	//编码
	fmt.Println(user.String())
	buffer, _ := proto.Marshal(user)
	
	fmt.Println(buffer)
	//解码
	var copy_user = &im.Person{}
	proto.Unmarshal(buffer, copy_user)
	
	fmt.Println("copy_user=%v\n", copy_user)
}
```

需要注意点：tutorail 文件要放在src目录下

---
参考资料：
- [golang protobuf 安装使用](https://my.oschina.net/u/988780/blog/676665)
- [Golang版protobuf的安装与使用](https://lihaoquan.me/2017/6/29/how-to-use-protobuf.html)
- [如何在go中使用protobuf](https://segmentfault.com/a/1190000010477733)
