# Demo of `ko`

`ko` is a little unknown gem, lost in the GitHub maze.

`ko` is widely used in [knative](https://github.com/knative) development but somehow very little advocacy has been done around this tool.

It allows you to:

* build Go binaries
* containerize them
* automatically update Kubernetes manifests to references the correct container image

All of this in one single command. 

## Install

With a working Go environment a simple `go get` will give you `ko`:

```
go get github.com/google/go-containerregistry/cmd/ko
```

## Usage

```
ko apply -f config/
```