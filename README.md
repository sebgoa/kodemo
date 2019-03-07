# Demo of `ko`

`ko` is a little unknown gem, lost in the GitHub maze.

`ko` is widely used in [knative](https://github.com/knative) development but somehow very little advocacy has been done around this tool.

It allows you to:

* build Go binaries
* containerize them and publish to a registry
* automatically update Kubernetes manifests to references the correct container image

All of this in one single command. 

## Install

With a working Go environment a simple `go get` will give you `ko`:

```
go get github.com/google/go-containerregistry/cmd/ko
```

## Configure a Registry

To specify the registry that you want to use, set the `KO_DOCKER_REPO` variable, for instance:

```
export KO_DOCKER_REPO=gcr.io/triggermesh
```

## Usage

Write a Kubernetes manifest with an import path that is similar to a Go import path like:

```
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: hello-world
spec:
...
  template:
...
    spec:
      containers:
      - name: hello-world
        image: github.com/sebgoa/kodemo/cmd/hello
```

Then to start the build, containerization and deployment a single `ko` command is necessary.

```
ko apply -f config/
```

You will see a Pod running and you will be able to call your Go function:

```
$ kubectl get pods
NAME                                            READY     STATUS      RESTARTS   AGE
hello-world-54557f6647-wsng6                    1/1       Running     0          50s

$ kubectl port-forward pod/hello-world-54557f6647-wsng6 8080:8080 &
[1] 99038

$ curl localhost:8080
Handling connection for 8080
Hello world !
```

## Build/publish but do not deploy

If all you want to do is build the Go binary and publish an image to the registry then, with the [Demo project](https://github.com/sebgoa/kodemo) cloned in your `$GOPATH`.

```
ko publish github.com/sebgoa/kodemo/cmd/hello
```

## Demo project

If you want to get started with `ko`, you can try the `kodemo` [repository](https://github.com/sebgoa/kodemo) is a simple demo project. It has the following structure:

```
$ tree
.
├── LICENSE
├── README.md
├── cmd
│   └── hello
│       └── hello.go
└── config
    └── deploy.yaml
```

You can certainly build your code locally:

```
go build cmd/hello/hello.go
```