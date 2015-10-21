hello_grpc_go
=============

helloworld grpc-go example copied from https://github.com/grpc/grpc-go/tree/master/examples/helloworld

## prerequisite

Install grpc. See [grpc/grpc-go](https://github.com/grpc/grpc-go) for README.

```
go get -u google.golang.org/grpc
```

Install protoc-gen-go. See [golang/protobuf](https://github.com/golang/protobuf) for README.

```
go get -u github.com/golang/protobuf/{proto,protoc-gen-go}
```

## master branch

This branch uses unreleased protobuf v3

### Install protobuf master

```
cd
git clone https://github.com/google/protobuf
cd protobuf
./autogen.sh
./configure --prefix=/usr/local/protobuf3
make
make install
```

### Generate helloworld.pb.go from helloworld.proto

```
cd helloworld
DYLD_LIBRARY_PATH=/usr/local/protobuf3/lib /usr/local/protobuf3/bin/protoc --go_out=plugins=grpc:. *.proto
```

## protobuf_v2 branch

This branch uses released version protobuf 2.6.1 protobuf

### Install protobuf 2.6.1

```
brew install protobuf
```

### Generate helloworld.pb.go from helloworld.proto

```
cd helloworld
protoc --go_out=plugins=grpc:. *.proto
```

