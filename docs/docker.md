# Docker

## 1. 什么是 Docker 以及为什么要用它：从服务部署的历史说起
为了更好地理解 Docker 的必要性，有必要讲一段历史。

最早，大家想要在服务器上部署一个服务进程，就是**直接运行程序**：例如想启动一个 nginx 服务器，就直接运行 `nginx` 命令。

后来，人们发现每个服务程序都有类似的操作：启动、停止、重启、查看日志等。这样，人们创立了 **服务（Service）** 的概念，在 Windows 上把 nginx 注册为一个 Service，就可以用 `sc start/stop nginx` 命令来启动/停止它，Linux 上则是 `service nginx start/stop`。

再后来，随着软件之间的关系逐渐变得复杂，运行一个服务也变得困难起来：php 可能依赖特定版本的 mysql 服务器软件，而 mysql 服务器软件的这个版本又不能在最新的系统中运行，但另一个软件 nginx 偏偏又需要最新的系统才能运行。这个时候，在同一个服务器上就没办法同时运行多个软件。好在天无绝人之路，**虚拟机（Virtual Machine）** 就可以解决这个问题。创建多个虚拟机，在每个虚拟机中安装各个服务软件和它们需要的依赖程序版本，就可以互不干扰地运行多个服务了。

再再后来，人们发现：根本没有必要创建一个完整的虚拟机啊！一个虚拟机会模拟出从底层硬件到操作系统的所有东西，而我们只是想要隔离不同版本的软件，这样的话，完全可以只隔离软件的运行环境，而不需要模拟硬件和操作系统。这就是 **容器（Container）** 的概念。现代意义上的容器首先由 FreeBSD 操作系统实现，BSD 在内核中添加了一组新的功能，统称为 Jails，形象地描述把软件关在隔离「监狱」里的情形。随后很快 Linux 跟进了 Linux VServer 技术，并演进出了 LXC（Linux Containers），可以实现不同环境的隔离。每个环境就是一个容器，容器中的软件可以运行在自己的环境中，只能看到自己需要的软件、文件和网络，而不会影响到其他容器。

容器非常轻量，和虚拟机相比，几乎没有额外的开销，也不像虚拟机在启动、停止时需要几分钟的时间，容器可以在几秒钟内启动、停止。

另一个问题是，人们又发现：很多时候大家想运行的容器是类似的。例如，几乎全世界的后端都要运行一个网关软件，这个软件大概率是 nginx。懒惰的人们不想每次都重新配置 nginx，于是就有了 **镜像（Image）** 的概念。镜像是一个压缩文件，里面包含了运行一个容器所需要的所有文件和配置，相当于「一键懒人包」。只要有了镜像，就可以在任何地方立刻运行一个一模一样的 nginx 容器，而不需要重新配置了。

最终，随着互联网和云概念的普及，人们发现：**镜像的分享** 是一个非常重要的功能。如果我在我的电脑上配置好了一个 nginx 容器，我希望能够把这个容器分享给其他人，让他们也能在自己的电脑上运行这个容器。最终，我们的 **Docker** 闪亮登场：Docker 是一个开源的容器引擎，可以让你随时运行、分享、修改、上传、下载容器；**Docker Hub** 是一个 Docker 镜像的仓库，你可以在上面找到几乎所有服务器软件的镜像。

今天，**Docker 已经不仅仅是后端运维人员偷懒的工具，它已经成为了云计算、微服务、DevOps 等概念的基石**，许多普通软件也会提供打包好的 Docker 镜像，让用户可以在任何地方运行。使用 Docker 可以极大地便利你的开发、测试、部署工作，无需顾虑环境配置问题。

> **你知道吗？**
>
> 早期的 Docker 依赖于 LXC，但后来 Docker 自己开发了一个更轻量的容器技术和一套镜像规范，称为 libcontainer。

> **Docker 并不「纯粹」？**
>
> Docker 最初只是一个开源项目，但今天已经成为了一个名为 Docker Inc. 的公司产品（由 Moby Project 社区运行），并且推出了一系列附属商业产品，例如 Docker Enterprise。它曾经有过许多争议：[Docker Hub 擅自停止免费 Team 计划并删除用户镜像，引发社区不满](https://www.docker.com/blog/we-apologize-we-did-a-terrible-job-announcing-the-end-of-docker-free-teams/)。
>
> 如果你并不喜欢 Docker Inc. 的商业策略，所幸 Docker 的遗产已经成就了开放容器标准 OCI（Open Container Initiative），例如 RedHat 公司的 [Podman](https://podman.io/) 等兼容 Docker 镜像的开源项目正在欣欣向荣地发展。我个人已经不使用 Docker 而是 Podman 很久了，体验也非常好。

## 2. 几个关于 Docker 的基本事实

本节解答一些关于 Docker 具体情况的疑惑。如果你仔细阅读了前一节，这些问题应该可以跳过。

1. **Docker 是什么？**
   Docker 是一个开源的容器引擎，可以让你随时运行、分享、修改、上传、下载容器。Docker 本身还是一个命令，可以在终端中输入 `docker` 命令来操作容器。
2. **Docker 的容器和普通的虚拟机有什么区别？**
   虚拟机是模拟出一个完整的计算机环境，包括硬件、操作系统和软件，而容器只是隔离出一个软件运行环境。虚拟机需要额外的开销，而容器几乎没有额外开销。
3. **Docker 能让我运行什么？**
   Docker 几乎可以运行任何 Linux 上的服务器软件，例如 nginx、mysql、redis 等。
4. **Docker 镜像是什么？**
   镜像是一个压缩文件，里面包含了运行一个容器所需要的所有文件和配置。镜像可以用来创建容器。
5. **Docker Hub 是什么？**
    Docker Hub 是一个 Docker 镜像的仓库，你可以在上面找到几乎所有服务器软件的镜像。你也可以上传自己的镜像到 Docker Hub 让别人下载。
6. **Docker 在 Windows 上也可以装吗？它如何运行 Linux 容器？**
   Docker 在 Windows 上有 Docker Desktop。Docker Desktop 使用了 Windows 10 中的 WSL（Windows Subsystem for Linux）技术，即虚拟机来运行 Linux 容器。因此，Docker Desktop 在 Windows 上运行的容器都是运行在一个 Linux 虚拟机中的。
7. **镜像不是一个文件吗，什么叫「启动一个镜像」？**
   一个镜像是一个压缩文件，你可以把它想象成一个软件的安装包。启动一个镜像，就是把这个安装包解压，然后运行其中指定好的特定程序。

## 3. 在个人电脑上安装 Docker

本节简单扼要介绍如何在自己的电脑上安装 Docker，更具体地说是 Docker Desktop。**你应当具有阅读英文的基本能力。**

1. 前往 Docker 官网：<https://www.docker.com/>；
2. 在 Product 菜单中找到 Docker Desktop，点击；
3. 点击页面中的 Download，下载 Docker Desktop 安装程序；
4. 双击安装程序，按照提示安装 Docker Desktop。

安装完成后，你需要从开始菜单中启动 Docker Desktop，右下角会出现一个鲸鱼图标，表示 Docker Desktop 正在运行。

你可以在终端中输入 `docker --version` 来检查 Docker 是否安装成功。我们接下来所有工作都将在终端中进行。

## 4. 在 Linux 服务器上安装 Docker

本节简单扼要介绍如何在 Linux 服务器上安装 Docker。如果你不需要了解，可以跳过。**你应当具有阅读英文的基本能力。**

服务器上安装的是 Docker Engine 而非 Desktop。

1. 在官网文档中选择 Engine -> Install：<https://docs.docker.com/engine/install/>；
2. 按照页面说明，选择对应的发行版。下面以 Ubuntu 为例：<https://docs.docker.com/engine/install/ubuntu/>；
3. 卸载已有的 Docker 软件：

``` bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

4. 安装 Docker Engine：

``` bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 5. 简单使用

Docker 的使用非常简单，只需要记住几个命令就可以了。我们以运行 MySQL 为例，介绍运行容器的全过程。

1. **搜索镜像**：在 [Docker Hub](https://hub.docker.com/) 中搜索「MySQL」，优先选择带有星标（Docker 官方维护）、下载量高、最后更新时间较近的镜像。这里满足以上要求的仅有官方镜像 <https://hub.docker.com/_/mysql>。

2. **下载镜像并启动容器**：

```bash
docker run --detach --name mysql --env MYSQL_ROOT_PASSWORD=123456 mysql:latest
```

解释：

- `docker run ... mysql:latest`：下载镜像、创建一个新容器并运行。中间 `...` 是运行参数，`mysql:latest` 指定镜像名，格式说明见下；
- `--detach`（或简写为 `-d`）：后台运行容器；如果不加，当前终端将被 mysql 占用，直到关闭容器。加上后命令会在容器启动后立刻返回，并在后台静默运行容器；
- `--name mysql`：给新建的容器设定名称。如果不加，docker 会自动分配一个随机名称；
- `--env MYSQL_ROOT_PASSWORD=123456`：设置容器的环境变量，这里是将 MySQL 的 root 用户密码设为 123456。**这个参数需要按照镜像的要求设置，不同镜像有不同的环境变量名**。

总之，我们在后台运行了一个容器，这个容器的初始镜像是 `mysql:latest`，容器名为 mysql，MySQL 数据库的 root 用户密码为 123456。

3. **查看容器状态**：

用以下命令列出所有容器：

```bash
docker ps -a
```

这里 `-a` 代表列出所有容器，包括已经停止的容器，如果不加则只列出正在运行的容器。你会看到一个名为 mysql 的容器，状态为 Up，表示正在运行。

4. **进入容器**：

```bash
docker exec -it mysql bash
```

这里 `-it` 是两个参数的简便写法。`-i`（或 `--interactive`）表示交互式操作，`-t`（或 `--tty`）表示分配一个伪终端，两者合并起来表示分配一个临时终端，并在接下来进入该终端。后面的 `bash` 代表执行 `bash` 命令，进入 mysql 容器的 bash。

容器可以看成一个微型的 Linux 虚拟机，可以用 Linux 命令浏览。你可以在这里输入 `mysql -u root -p` 来进入 MySQL 数据库，或者 `exit` 退出容器。

5. **查看日志**：

```bash
docker logs mysql
```

非常好理解，就是输出 mysql 容器的日志。

6. **停止容器**：

```bash
docker stop mysql
```

这里 `mysql` 是容器名，你也可以用容器 ID 来指定容器。已经停止的容器还可以重新用 `docker start mysql` 启动，如果你想进一步删除容器，可以用 `docker rm mysql`。

### 5.1. 镜像名格式

在上面的例子中，我们用到了 `mysql:latest`，这是一个镜像名。镜像名的格式是 `[源地址]/[仓库名]/[镜像名]:[标签]`，其中：

- **源地址**：表示镜像从哪个仓库服务器下载。Docker Hub 是最大的镜像仓库，如果不指定源地址，默认为 Docker Hub。也有很多其他镜像仓库，例如 GitHub 的 ghcr.io 或 RedHat 的 quay.io；
- **仓库名**：表示镜像的来源，可以是 Docker Hub 的用户名、组织名，或者其他镜像仓库的地址。对于 Docker Hub 中网址含有 `_`（例如 `hub.docker.com/_/mysql`）的官方镜像，仓库名和后面的 `/` 可以省略；
- **镜像名**：表示镜像的名称，例如 `mysql`；
- **标签**：表示镜像的版本。同一个镜像可能有多个版本，例如你可能需要特定版本的 MySQL，或者希望这个容器中运行的是特定发行版的 Linux。如果不指定标签，默认为 `latest`。

> **注意**
>
> 标签并没有严格的规定，发布的镜像可以有任意多个标签。`latest` 只是一个约定俗成的标签，表示最新版本。如果你省略标签，Docker 会默认使用 `latest`。
>
> 和环境变量一样，你需要仔细阅读 Docker Hub 上镜像的说明，了解镜像的标签和环境变量可以取什么值。

一些例子：

- `mysql:latest`：Docker Hub 上的 MySQL 官方镜像的最新版本；
- `ubuntu:20.04`：Docker Hub 上的 Ubuntu 官方镜像的 20.04 版本；
- `nginx`：Docker Hub 上的 Nginx 官方镜像的最新版本；
- `myname/myimage:1.0`：Docker Hub 上的 myname 用户的 myimage 镜像的 1.0 版本；
- `ghcr.io/myname/myimage`：GitHub Container Registry 上的 myname 用户的 myimage 镜像的最新版本。

## 6. docker-compose

手写 Docker 命令行也是一件痛苦的事。除了上面的 `--env` 参数，还有很多参数需要记住。这样一来，使用 Docker 不又变得和 LXC 一样麻烦了吗？

为了简化这个过程，Docker 提供了一个叫做 `docker-compose` 的工具，可以用一个 YAML 文件来描述容器的配置，然后用 `docker compose` 命令来启动容器。

> **YAML 文件**：
>
> YAML 是一种简单的数据格式，类似于 JSON，但更易读。YAML 文件以 `.yml` 或 `.yaml` 为后缀，可以用文本编辑器打开。YAML 文件中的每一行都是一个键值对，键和值之间用冒号分隔，键值对之间用空格分隔。例如：
>
> ```yaml
> key1: value1
> key2: value2
> key3:
>   - value3
>   - value4
> ```
>
> 这个文件中有三个键值对，`key1` 的值是 `value1`，`key2` 的值是 `value2`，`key3` 的值是一个列表，包含两个元素 `value3` 和 `value4`。
> 
> YAML 文件中要注意缩进，缩进表示层级关系。例如：
>
> ```yaml
> key1:
>   key2: value2
>   key3: value3
> ```
>
> 这里 `key2` 和 `key3` 是 `key1` 的子键，它们的值是 `value2` 和 `value3`。
> 
> 你可以在 [YAML 语言教程](https://ruanyifeng.com/blog/2016/07/yaml.html) 查看更多关于 YAML 的信息。

一个典型的 `docker-compose.yml` 文件如下：

```yaml
version: '3.1'

services:
  mysql:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: 123456
```

这个文件描述了一个服务 `mysql`（「服务」是 Docker Compose 的概念，和 Docker 本身的「容器」类似），这个服务的各项设置和上面的 `docker run` 命令一样。

创建这个文件后，在包含这个文件的目录下，你可以用 `docker compose up` 命令来启动这个服务，用 `docker compose down` 命令来停止这个服务。

`docker-compose` 的优势在于，你可以在一个文件中描述多个服务之间的关系，例如一个服务依赖另一个服务，或者一个服务需要另一个服务的输出。这样，你可以用一个命令启动整个应用，而不需要一个一个启动服务。

举例，你需要一个 MySQL 服务和一个 PHP 服务，PHP 服务需要 MySQL 服务。你可以在 `docker-compose.yml` 文件中描述这两个服务，然后用 `docker compose up` 一次性启动这两个服务：

```yaml
version: '3.1'

services:
  mysql:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: 123456

  php:
    image: php:latest
    depends_on:
      - mysql
```

这里 `php` 服务依赖于 `mysql` 服务，`docker compose up` 命令会先启动 `mysql` 服务，然后再启动 `php` 服务，也就是启动两个不同的容器。同理，`docker compose down` 命令会先停止 `php` 服务，再停止 `mysql` 服务。
