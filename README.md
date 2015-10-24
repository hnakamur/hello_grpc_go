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

### Generate API documents

#### Install protoc-gen-doc

I installed [estan/protoc-gen-doc](https://github.com/estan/protoc-gen-doc) with the following steps.

```
brew install protobuf

brew install qt5
brew link qt5 --force

export PROTOBUF_PREFIX=/usr/local/Cellar/protobuf/2.6.1
qmake
make
make install
```

Also install packages to build PDF files from DocBook files.

```
brew install fop docbook-xsl
```

#### Build API documents

Generate API documents with the following steps.

```
brew install fop docbook-xsl

cd "$GOPATH/src/github.com/hnakamur/hello_grpc_go"
git checkout protobuf_v2
mkdir doc
protoc --doc_out=html,index.html:doc helloworld/*.proto
protoc --doc_out=markdown,helloworld.md:doc helloworld/*.proto
protoc --doc_out=docbook,helloword.docbook:doc helloworld/*.proto

DOCBOOK_XSL=/usr/local/Cellar/docbook-xsl/1.78.1_1/docbook-xsl-ns/fo/docbook.xsl
fop -xml doc/helloworld.docbook \
    -xsl "$DOCBOOK_XSL" \
    -param use.extensions 0 \
    -param fop1.extensions 1 \
    -param paper.type A4 \
    -param page.orientation landscape \
    -pdf doc/helloworld.pdf
```

You can see generated API documents:

* [index.html](http://hnakamur.github.io/hello_grpc_go/doc/)
* [helloworld.md](https://github.com/hnakamur/hello_grpc_go/blob/protobuf_v2/doc/helloworld.md)
* [helloworld.docbook](https://raw.githubusercontent.com/hnakamur/hello_grpc_go/protobuf_v2/doc/helloworld.docbook)
* [helloworld.pdf](http://hnakamur.github.io/hello_grpc_go/doc/helloworld.pdf)
