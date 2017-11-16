---
layout: post
title: Go用反射动态将json转为proto消息
date: 2017-09-11 20:20:20
categories:
- golang
- 语言与设计
tags:
---

golang标准库有reflect包，同Java类似具有反射特性。

通过msgType = reflect.TypeOf(msg.(proto.Message))获取proto类型，把interface类型和指定消息id注册到一个自定义map中。

使用时通过指定具体的消息id，由reflect.New(info.msgType.Elem()).Interface().(proto.Message)即类型的Elem()函数得到指针的基类型，来获取相应的proto消息，再通过jsonpb.UnmarshalString(msgJson, nmsg)将json映射到对于proto消息上。

gosim启动命令：

```
gosim -h
Usage of gosim:
  -file string
        json file (default "./msgjson/OpenReq.json")
  -h    help
  -ipport string
        ip:port (default "localhost:5322")
  -msg string
        proto message name (default "OpenReq")
```


gosim.go 实现程序框架，加载json文件，调用反射组proto包，socket连接服务器发送proto报文。

```golang
//gosim.go

package main

import (
    "fmt"
    "net"
    "flag"
    "io/ioutil"
    
    "msgreflect"
)

var help = flag.Bool("h", false, "help")
var msg = flag.String("msg", "OpenReq", "proto message name")
var file = flag.String("file", "./msgjson/OpenReq.json", "json file")
var ipport = flag.String("ipport", "localhost:5322", "ip:port")

func LoadInputFile(filename string) (content string, err error) {
    fileContentBytes, err := ioutil.ReadFile(filename)
    if err != nil {
        return
    }
    content = string(fileContentBytes[:])
    return
}

func main() {
    flag.Parse()

    if *help {
        flag.Usage()
        return
    }

    msgJson, err := LoadInputFile(*file)
    if err != nil {
        fmt.Printf("Load json file error: %s\n", err)
        return
    }

    //dynamic parse Json to protobuf message.
    msgreflect.RegistMsg()
    head_data, req_data, _ := msgreflect.PackageMessage(uint32(msgreflect.MsgMap[*msg]), msgJson)
    
    //send proto message to socket.
    conn, err := net.Dial("tcp", *ipport)
    if err != nil {
    	fmt.Println("dial error:", err.Error())
    	return
    }
    _, err = conn.Write(head_data)
    if err != nil {
    	fmt.Println("write head error:", err.Error())
    	return
    }
    _, err = conn.Write(req_data)
    if err != nil {
    	fmt.Println("write req error:", err.Error())
    	return
    }

    fmt.Println("send msg ok.", *ipport)
    conn.Close()
}
```

msgreflect.go 实现反射功能。

```golang
//msgreflect.go

package msgreflect

import (
	"fmt"
	"errors"
	"reflect"

	"github.com/golang/protobuf/jsonpb"
	"github.com/golang/protobuf/proto" 
)

//type MessageHandler func(msgid uint16, msg interface{}) (nmsg proto.Message, command uint32)

type MessageInfo struct {
    msgType    reflect.Type
    //msgHandler MessageHandler
}

var (
    msg_map = make(map[uint32]MessageInfo)
)

func RegisterMessage(msgid uint32, msg interface{}) {
    var info MessageInfo
    info.msgType = reflect.TypeOf(msg.(proto.Message))
    //info.msgHandler = handler
    msg_map[msgid] = info
}

func ParseMessage(msgid uint32, msgJson string) (nmsg proto.Message, err error) {
    if info, ok := msg_map[msgid]; ok {
        nmsg = reflect.New(info.msgType.Elem()).Interface().(proto.Message)
        //msg := reflect.New(info.msgType.Elem()).Interface()
        //nmsg = info.msgHandler(msgid, msg)
        err = jsonpb.UnmarshalString(msgJson, nmsg)
        if err != nil {
            fmt.Printf("Unmarshal msgJson to pb request error:%s\n", err)
            return
        }
        return
    } else {
        err = errors.New("not found msgid")
        return 
    }    
}
```

msghandler.go 实现proto消息反射注册。

```golang
//msghandler.go
package msgreflect

import (
	"fmt"
	"bytes"
	"encoding/binary"

	"trademsg"
	"github.com/golang/protobuf/proto"
)

type PacketHead struct {
    Version uint32
    Length uint32
    Command uint32
}

/*******************************************************
    --修改处开始--
*******************************************************/
//1.新增消息名称映射及命令码
var MsgMap = map[string]uint32 {
        "OpenReq": 0X010101,
}

//2.注册新增proto消息映射，包括命令码和对应protobuf消息
func RegistMsg() {
	RegisterMessage(0X010101, &trademsg.OpenReq{})
}
/*******************************************************
    --修改处结束--
*******************************************************/

func PackageMessage(msgid uint32, msgJson string) (head_data []byte, req_data []byte, err error) {
    req, err := ParseMessage(msgid, msgJson)
    if err != nil {
        fmt.Println("ParseMessage error:", err.Error())
        return
    }
    req_data, err = proto.Marshal(req)
    if err != nil {
        fmt.Println("proto Marshal error:", err.Error())
        return
    }

    head := PacketHead {
    	Version: 1,
    	Length: uint32(32 + len(req_data)),
    	Command: msgid,
    }     
    head_buf := new(bytes.Buffer)
	binary.Write(head_buf, binary.BigEndian, head)
	head_data = head_buf.Bytes()
    
    fmt.Printf("msg_head:\n%+v\n\n", head)
    fmt.Printf("msg_body:\n{ %+v}\n\n", req)

	return
}
```

```json
//json消息
{
  "SID": "sid_test2",
  "UserID": 10001,
}
```

```proto
//proto消息
message OpenReq{
  required string   SID = 1;
  required int32    UserID = 2;
}
```
