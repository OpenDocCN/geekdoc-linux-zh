# 容器

`docker`和`podman`都是容器化工具。docker 镜像托管在[`hub.docker.com/r/curlimages/curl`](https://hub.docker.com/r/curlimages/curl)

您可以使用以下命令运行 curl 的最新版本：

`docker`的命令：

```sh
docker run -it --rm docker.io/curlimages/curl www.example.com
```

`podman`的命令：

```sh
podman run -it --rm docker.io/curlimages/curl www.example.com
```

## 在容器中无缝运行 curl

可以创建一个别名，以便在容器中无缝运行 curl，就像它在宿主操作系统上安装的原生应用程序一样。

在 Bash、ZSH、Fish shell 中将 curl 定义为容器化工具别名的命令：

### Bash 或 zsh

使用`docker`调用 curl：

```sh
alias curl='docker run -it --rm docker.io/curlimages/curl'
```

使用`podman`调用 curl：

```sh
alias curl='podman run -it --rm docker.io/curlimages/curl'
```

### 鱼类

使用`docker`调用 curl：

```sh
alias -s curl='docker run -it --rm docker.io/curlimages/curl'
```

使用`podman`调用 curl：

```sh
alias -s curl='podman run -it --rm docker.io/curlimages/curl'
```

简单调用`curl www.example.com`来发送请求

## 在 kubernetes 中运行 curl

有时使用 curl 来排查 k8s 网络问题可能很有用，就像：

```sh
kubectl run -i --tty curl --image=curlimages/curl --restart=Never \
  -- "-m 5" www.example.com
```
