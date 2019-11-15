## Golang CGO based cross compiling
Go supports out of box cross compiling feature by changing GOOS to appropiate operating system. Eg GOOS=Windows OR GOOS=Darwin etc. Then, **why we require this docker image?** Go faces challenges while compiling third party libaries if it has C/C++ code base in it.

When we want to cross compile 3rd party packages such as SQLite  we would need to set a flag **CGO_ENABLED=1**, This flag tells golang to compile C/C++ snippets when those 3rd party packages. Most of the times it will fail to create package for target operating system. 

This docker file when build, will install all the required tool chains, C/C++ based cross compilers and its header files which are required to target below OS & architecture. Apart from tool chains, it will also build [golang 1.13.4](https://golang.org/doc/devel/release.html#go1.13 "Go1.13.4"), [Go Dep](https://golang.github.io/dep/docs/introduction.html "Dep") & [GoReleaser](https://goreleaser.com "Go Releaser") 

## Supported Platforms & Architectures
**Platforms:** darwin, linux, windows

**Achitectures:** 386, amd64, arm-5, arm-6, arm-7, arm64, mips, mipsle, mips64, mips64le

## Example Usage 
1) Build the docker image using below command
`$ docker build -t gocross .`

2) Check if image is created using below command
`$ docker images`
above command should show you if image was created successfuly or not. 

3) Login into the created image using below command
`$ docker run -it gocross /bin/bash`

You are ready to cross compile!

**Note:** 
Since we are building this image for mutiple architectures & Platforms build time for this docker image is on a higher side.