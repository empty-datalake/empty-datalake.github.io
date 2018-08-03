---
toc: true
toc_sticky: true
title: "Dockerfile"
date: 2018-08-03 16:00:00 +0900
categories: 
    - Docker 
tags: 
    - Docker
    - Dockerfile
---

## Intro

```Dockerfile``` 은 docker 이미지를 빌드하기 위한 명령어로 구성된 파일이다. ```docker build``` 명령어를 사용해서 이미지를 생성 할 수 있다. Docker로 무언가를 시작할때 가장 처음 접하게 되는 주제이다. 실제 사용하면서 알게된 내용을 하나씩 추가하면서 정리할 예정이다. 너무 많다.

## Version

이 문서를 작성하는 시점에서 사용하고 있는 docker version은 아래와 같다.

```bash
$ docker version
Client:
 Version:           18.06.0-ce
 API version:       1.38
 Go version:        go1.10.3
 Git commit:        0ffa825
 Built:             Wed Jul 18 19:09:54 2018
 OS/Arch:           linux/amd64
 Experimental:      false

Server:
 Engine:
  Version:          18.06.0-ce
  API version:      1.38 (minimum version 1.12)
  Go version:       go1.10.3
  Git commit:       0ffa825
  Built:            Wed Jul 18 19:07:56 2018
  OS/Arch:          linux/amd64
  Experimental:     false
```

## Usage

이 세상 대부분 명령어는 argument를 가지고 있다. **Docker** 또한 다르지 않다. CLI에서 명령어의 옵션들을 모른다면 언제나 *--help* 다. 다시 말하지만 한번에 아래의 모든 내용을 공부하진 않을 것이다. 많다.

```bash
$ docker build --help

Usage:	docker build [OPTIONS] PATH | URL | -

Build an image from a Dockerfile

Options:
      --add-host list           Add a custom host-to-IP mapping (host:ip)
      --build-arg list          Set build-time variables
      --cache-from strings      Images to consider as cache sources
      --cgroup-parent string    Optional parent cgroup for the container
      --compress                Compress the build context using gzip
      --cpu-period int          Limit the CPU CFS (Completely Fair
                                Scheduler) period
      --cpu-quota int           Limit the CPU CFS (Completely Fair
                                Scheduler) quota
  -c, --cpu-shares int          CPU shares (relative weight)
      --cpuset-cpus string      CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string      MEMs in which to allow execution (0-3, 0,1)
      --disable-content-trust   Skip image verification (default true)
  -f, --file string             Name of the Dockerfile (Default is
                                'PATH/Dockerfile')
      --force-rm                Always remove intermediate containers
      --iidfile string          Write the image ID to the file
      --isolation string        Container isolation technology
      --label list              Set metadata for an image
  -m, --memory bytes            Memory limit
      --memory-swap bytes       Swap limit equal to memory plus swap:
                                '-1' to enable unlimited swap
      --network string          Set the networking mode for the RUN
                                instructions during build (default "default")
      --no-cache                Do not use cache when building the image
      --pull                    Always attempt to pull a newer version of
                                the image
  -q, --quiet                   Suppress the build output and print image
                                ID on success
      --rm                      Remove intermediate containers after a
                                successful build (default true)
      --security-opt strings    Security options
      --shm-size bytes          Size of /dev/shm
  -t, --tag list                Name and optionally a tag in the
                                'name:tag' format
      --target string           Set the target build stage to build.
      --ulimit ulimit           Ulimit options (default [])
```

### Context

위 help의 **Usage** 를 보면 ```PATH```와 ```URL``` 을 볼 수 있다. 바로 context를 지정하는 것으로 ```PATH```는 로컬 파일시스템의 경로를 의미하고 ```URL```은 Git의 레포지토리를 의미한다. 빌드 과정에서 docker 데몬에게 전체 context가 recursive하게 전달되며 ```.dockerignore``` 파일을 이용해서 제외될 파일을 지정 할 수 있다. 성능적인 측면에서 빈 디렉토리를 지정하는 것이 제일 좋다. 별도로 지정하지 않는 경우 context의 루트에 포함된 ```Dockerfile```을 사용해서 이미지를 빌드한다.

> 만약 root 디렉토리인 ```/``` 를 ```PATH```로 지정하는 경우 로컬 파일시스템 전체가 docker 데몬에게 전달된다.

```bash
$ docker build .
```

### -f, --file *string*

```Dockerfile```의 위치를 지정한다. Context와 다른 위치의 ```Dockerfile```을 참조해서 이미지를 빌드 할 수 있다.

```bash
$ docker build -f /path/to/a/Dockerfile .
```

### -t, --tag *list*

이미지의 repository와 tag를 설정한다. ```-t```를 반복 사용해서 여러개를 한번에 설정 할 수 있다.

```bash
$ docker build -t shykes/myapp .
$ docker build -t shykes/myapp:1.0.2 -t shykes/myapp:latest .
```

### --build-arg *list*

```Dockerfile```에서 사용할 arguments를 전달한다. 전달된 arguments는 ```Dockerfile```안에서 ```ARG``` 명령어를 통해서 사용할 수 있다. 자세한 사항은 ```Dockerfile```의 명령어를 정리할 때 알아보자.

```bash
$ docker build --build-arg WAR_FILE=build/test.war .
```

### ```Dockerfile```에서 사용되는 명령어(Instruction)

```docker budil```를 통해서 ```Dockerfile```속 명령어를 수행하기전 docker는 ```Dockerfile```를 검사하고 문법이 틀린경우 오류를 반환한다.

```bash
$ docker build -t test/myapp .
Sending build context to Docker daemon 2.048 kB
Error response from daemon: Unknown instruction: RUNCMD
```

```Dockerfile```에 작성된 명령어들은 하나씩 순차적으로 실행되며 필요한 경우 실행결과를 포한한 이미지를 생성한다. ```docker build```의 속도를 높이기 위해서 필요한경우 캐쉬된 중간 이미지들은 사용한다.

```bash
$ docker build -t svendowideit/ambassador .
Sending build context to Docker daemon 15.36 kB
Step 1/4 : FROM alpine:3.2
 ---> 31f630c65071
Step 2/4 : MAINTAINER SvenDowideit@home.org.au
 ---> Using cache
 ---> 2a1c91448f5f
Step 3/4 : RUN apk update &&      apk add socat &&        rm -r /var/cache/
 ---> Using cache
 ---> 21ed6e7fbb73
Step 4/4 : CMD env | grep _TCP= | (sed 's/.*_PORT_\([0-9]*\)_TCP=tcp:\/\/\(.*\):\(.*\)/socat -t 100000000 TCP4-LISTEN:\1,fork,reuseaddr TCP4:\2:\3 \&/' && echo wait) | sh
 ---> Using cache
 ---> 7ea8aef582cc
Successfully built 7ea8aef582cc
```
## Format

기본적인 ```Dockerfile```의 형식은 아래와 같다.

```dockerfile
# Comment
INSTRUCTION arguments
```

명령어는 대소문자를 가리지 않으며, 뒤의 arguments와 구분을 쉽게 하기 위해서 대문자로 쓰는것이 일반적인다.

명령어들은 ```Dockerfile```에 작성된 순서대로 수행이되며 반드시 ```FROM``` 명령어로 시작이 되어야 한다. ```FROM``` 명령어는 새로 빌드할 이미지의 *base image*를 지정한다.

주석은 ```#``` 를 사용해서 작성한다. ```#```을 argument로써 사용 할 수 있으며 아래와 같다.

```dockerfile
# Comment
RUN echo 'we are running some # of cool things'
```

## .dockerignore file

```.dockerignore``` 파일을 통해서 context의 파일 줄 일부를 제외할 수 있다. 익숙한 형식이니 설명은 그만하고 샘플은 아래와 같다. ```!```를 앞에 붙히는 경우에는 허용 목록이다.

```
# comment
*/temp*
*/*/temp*
temp?
```

## FROM

```dockerfile
FROM <image> [AS <name>]
```
```dockerfile
FROM <image>[:<tag>] [AS <name>]
```
```dockerfile
FROM <image>[@<digest>] [AS <name>]
```

Docker 이미지 빌드의 초기화를 담당하는 명령어로 *Base Image*를 지정한다.  ```Dockerfile``` 파일은 반드시 ```FROM```으로 시작해야 한다. 

- ```ARG```는 ```FROM``` 앞에 위치할 수 있다.
- ```FROM```은 여러번 나올 수 있다. 하지만 최종 이미지는 마지막에 나온 ```FROM```을 기반으로 만들어진다.

### ARG와 FROM의 상호작용에 대한 이해

```Dockerfile```은 ```FROM```으로 시작해야 한다. 하지만 ```ARG```는 ```FROM```앞에 올 수 있다. 다음 예를 보자.

```dockerfile
ARG  CODE_VERSION=latest
FROM base:${CODE_VERSION}
CMD  /code/run-app

FROM extras:${CODE_VERSION}
CMD  /code/run-extras
```

```FROM``` 앞에서 사용한 ```ARG```는 빌드 과정에서 다시 사용할 수 없다. 만약 해당 ```ARG```의 값을 사용하기 위해서는 값이 없는 ```ARG```를 다시 선언해서 사용한다. 

```dockerfile
ARG VERSION=latest
FROM busybox:$VERSION
ARG VERSION
RUN echo $VERSION > image_version
```

## RUN

```RUN```은 두 가지 형태를 가진다.

- ```RUN <command>``` (*shell* form)
- ```RUN ["executable", "param1", "param2"]``` (*exec* form)

```RUN``` 명령어는 현재 이미지에서 다른 명령어(command)를 실행하며 그 결과를 이미지에 반영한다. 그 결과는 ```Dockerfile```의 다음 단계에서 사용된다.

```SHELL``` 명령어를 통해서 기본 *shell*을 지정하거나 변경할 수 있다.

```\```를 사용해서 하나의 ```RUN```에 여러줄의 명령어(command)를 작설할 수 있다.

```dockerfile
RUN /bin/bash -c 'source $HOME/.bashrc; \
echo $HOME'
```

위 예를 한 줄로 작성하면 아래와 같다.

```dockerfile
RUN /bin/bash -c 'source $HOME/.bashrc; echo $HOME'
```

```RUN``` 명령어의 캐쉬는 다음 빌드에서 자동으로 사라지지 않고 영향을 미친다. 만약 ```RUN apt-get dist-upgrade -y```와 같은 명령어가 캐쉬되어 있다면 다음 빌드에서는 그 캐쉬를 재 사용하게 된다. 이를 피하기 위해서는 ```--no-cache``` 옵션을 사용해야 한다.

```bash
docker build --no-cache
```

```RUN``` 명령어의 캐쉬는 ```ADD``` 명령어에 의해서 제거 될 수 있다.

## CMD

```CMD```는 세 가지 형태를 가진다.

- ```CMD ["executable","param1","param2"]``` (*exec* form, this is the preferred form)
- ```CMD ["param1","param2"]``` (as default parameters to *ENTRYPOINT*)
- ```CMD command param1 param2``` (*shell* form)

```CMD```는 ```Dockerfile```에서 유일해야 한다. 만약 여러개의 ```CMD```를 사용하면 마지막의 ```CMD```만 동작한다. 

```CMD```의 주 목적은 컨테이너가 실행될때의 기본 동작의 정의하기 위함이다. 

만약 *shell* 혹은 *exec* form을 사용하는 경우 ```CMD``` 명령어는 구동중인 이미지 위에서 지정된 명령어(command)를 실행한다. 

*shell* form을 사용하는 경우에는 ```<commands>```는 ```/bin/sh -c```를 통해서 실행된다.

```dockerfile
FROM ubuntu
CMD echo "This is a test." | wc -
```

*shell* 없이 ```<commands>```를 실행하고 싶은 경우에는 *exec* form을 사용해야하며 더 선호되는 방법이다.

```dockerfile
FROM ubuntu
CMD ["/usr/bin/wc","--help"]
```

컨테이너 안에서 매번 동일한 명령어를 수행하고 싶은 경우에는 ```ENTRYPOINT```를 고려하자.

> ```RUN```과 ```CMD```를 혼동하기 쉽다. ```RUN```은 명령을 실행하고 이미지에 적용한다. 하지만 ```CMD```는 빌드시 아무 것도 실행하지 안지만 의도된 명령어를 이미지 안에 저장한다.


## LABEL

```dockerfile
LABEL <key>=<value> <key>=<value> <key>=<value> ...
```

```LABEL```은 메타데이터응 이미지에 저장하기 위해서 사용하는 명령어이다. 키-값을 가지는 구조로 되어 있으며 공백을 토함하기 위해서는 큰따옴표를 사용한다. ```\```를 사용해서 줄바꿈을 할 수 있다. 한번에 여러 레이블을 지정할 수 있으며 부모 이미지에서 지정된 레이블은 상속된다. 만약 자식이 동일한 key를 가지는 레이블을 선언하면 값을 덮어쓴다.

이미지의 레이블은 ```docker inspect```로 확인 할 수 있다.

```dockerfile
LABEL "com.example.vendor"="ACME Incorporated"
LABEL com.example.label-with-value="foo"
LABEL version="1.0"
LABEL description="This text illustrates \
that label-values can span multiple lines."

LABEL multi.label1="value1" multi.label2="value2" other="value3"

LABEL multi.label1="value1" \
      multi.label2="value2" \
      other="value3"
```


## References

- [Dockerfile reference](https://docs.docker.com/engine/reference/builder/#usage)