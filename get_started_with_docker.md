# 在Ubuntu上安装Docker

[官方文档地址](https://docs.docker.com/engine/install/ubuntu/)

### 查看本机信息

```shell
$ uname -a
```

```
Linux ubuntu 5.11.0-18-generic #19-Ubuntu SMP Fri May 7 14:22:03 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
```

### 卸载老的版本

```shell
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```

我们没安装过 Docker，所以显示

```
正在读取软件包列表... 完成
正在分析软件包的依赖关系树... 完成
正在读取状态信息... 完成                 
E: 无法定位软件包 docker-engine
```

### 安装 Docker

我们有多种方法安装 Docker：

- 推荐的方法。设置 Docker仓库，便于在线安装和升级。
- 如果您的电脑无法访问网络，可以下载deb包，在本地安装或升级。
- 测试开发环境下，可以选择脚本快捷安装。

#### 使用仓库安装

如果您在一台新机器上安装 Docker 引擎，您需要先设置 Docker仓库。然后才能从仓库中安装docker。

##### 设置仓库

1. 更新 `apt` 包索引，并从 `apt` 安装一些与 HTTPS 相关的包

   ```shell
    $ sudo apt-get update
    $ sudo apt-get install \
       apt-transport-https \
       ca-certificates \
       curl \
       gnupg \
       lsb-release
   ```

   更新 `apt` 结果：

   ```
   命中:1 http://dl.google.com/linux/chrome/deb stable InRelease
   命中:2 http://archive.canonical.com/ubuntu hirsute InRelease                 
   命中:3 http://archive.ubuntu.com/ubuntu hirsute InRelease                   
   命中:4 http://cz.archive.ubuntu.com/ubuntu focal InRelease                   
   获取:5 http://archive.ubuntu.com/ubuntu hirsute-updates InRelease [109 kB]   
   ......                          
   命中:12 https://apt.atzlinux.com/atzlinux buster InRelease                   
   命中:13 http://archive.ubuntu.com/ubuntu hirsute-backports InRelease         
   命中:14 http://archive.ubuntu.com/ubuntu hirsute-security InRelease
   已下载 109 kB，耗时 2秒 (43.6 kB/s)
   正在读取软件包列表... 完成
   ```

   安装升级多个需要的包：

   ```
   正在读取软件包列表... 完成
   正在分析软件包的依赖关系树... 完成
   正在读取状态信息... 完成                 
   ca-certificates 已经是最新版 (20210119build1)。
   ca-certificates 已设置为手动安装。
   curl 已经是最新版 (7.74.0-1ubuntu2)。
   curl 已设置为手动安装。
   gnupg 已经是最新版 (2.2.20-1ubuntu3)。
   gnupg 已设置为手动安装。
   lsb-release 已经是最新版 (11.1.0ubuntu2)。
   lsb-release 已设置为手动安装。
   下列【新】软件包将被安装：
     apt-transport-https
   升级了 0 个软件包，新安装了 1 个软件包，要卸载 0 个软件包，有 9 个软件包未被升级。
   需要下载 1,704 B 的归档。
   解压缩后会消耗 166 kB 的额外空间。
   您希望继续执行吗？ [Y/n] y
   获取:1 http://archive.ubuntu.com/ubuntu hirsute/universe amd64 apt-transport-https all 2.2.3 [1,704 B]
   已下载 1,704 B，耗时 1秒 (3,012 B/s)            
   正在选中未选择的软件包 apt-transport-https。
   (正在读取数据库 ... 系统当前共安装有 199929 个文件和目录。)
   准备解压 .../apt-transport-https_2.2.3_all.deb  ...
   正在解压 apt-transport-https (2.2.3) ...
   正在设置 apt-transport-https (2.2.3) ...
   ```

2. 添加 Docker 官方 GPG key

   ```shell
   $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

   什么也没输出

3. 设置标准版（stable）安装仓库，如果需要安装 nightly 版本或 test 版本，替换一下关键字即可：

   ```shell
   $ echo \
     "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
     $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

   同样没有任何输出

##### 安装 Docker 引擎

1. 更新 `apt` 包索引，安装 Docker 引擎和 containerd 最新版本，如果想安装特定版请看下一步：

   ```shell
   $ sudo apt-get update
   $ sudo apt-get install docker-ce docker-ce-cli containerd.io
   ```

   更新 `apt` 输出结果：

   ```
   命中:1 http://dl.google.com/linux/chrome/deb stable InRelease
   命中:2 https://linux.teamviewer.com/deb stable InRelease                    
   命中:3 http://archive.canonical.com/ubuntu hirsute InRelease                 
   命中:4 http://cz.archive.ubuntu.com/ubuntu focal InRelease                   
   命中:5 http://archive.ubuntu.com/ubuntu hirsute InRelease                   
   获取:6 https://download.docker.com/linux/ubuntu hirsute InRelease [32.2 kB]
   获取:7 http://archive.ubuntu.com/ubuntu hirsute-updates InRelease [109 kB]
   获取:8 https://download.docker.com/linux/ubuntu hirsute/stable amd64 Packages [3,434 B]
   ......
   命中:16 https://apt.atzlinux.com/atzlinux buster InRelease
   已下载 144 kB，耗时 4秒 (36.7 kB/s)
   正在读取软件包列表... 完成
   ```

   安装 Docker 等包结果：

   ```
   正在读取软件包列表... 完成
   正在分析软件包的依赖关系树... 完成
   正在读取状态信息... 完成                 
   将会同时安装下列软件：
     docker-ce-rootless-extras docker-scan-plugin libslirp0 pigz slirp4netns
   建议安装：
     aufs-tools cgroupfs-mount | cgroup-lite
   下列【新】软件包将被安装：
     containerd.io docker-ce docker-ce-cli docker-ce-rootless-extras
     docker-scan-plugin libslirp0 pigz slirp4netns
   升级了 0 个软件包，新安装了 8 个软件包，要卸载 0 个软件包，有 9 个软件包未被升级。
   需要下载 108 MB 的归档。
   解压缩后会消耗 466 MB 的额外空间。
   您希望继续执行吗？ [Y/n] y
   获取:1 http://archive.ubuntu.com/ubuntu hirsute/universe amd64 pigz amd64 2.6-1 [63.6 kB]
   获取:2 https://download.docker.com/linux/ubuntu hirsute/stable amd64 containerd.io amd64 1.4.6-1 [28.3 MB]
   获取:3 http://archive.ubuntu.com/ubuntu hirsute/main amd64 libslirp0 amd64 4.4.0-1 [55.9 kB]
   获取:4 http://archive.ubuntu.com/ubuntu hirsute/universe amd64 slirp4netns amd64 1.0.1-1 [33.1 kB]
   获取:5 https://download.docker.com/linux/ubuntu hirsute/stable amd64 docker-ce-cli amd64 5:20.10.7~3-0~ubuntu-hirsute [41.4 MB]
   获取:6 https://download.docker.com/linux/ubuntu hirsute/stable amd64 docker-ce amd64 5:20.10.7~3-0~ubuntu-hirsute [24.8 MB]
   获取:7 https://download.docker.com/linux/ubuntu hirsute/stable amd64 docker-ce-rootless-extras amd64 5:20.10.7~3-0~ubuntu-hirsute [9,062 kB]
   获取:8 https://download.docker.com/linux/ubuntu hirsute/stable amd64 docker-scan-plugin amd64 0.8.0~ubuntu-hirsute [3,889 kB]
   已下载 108 MB，耗时 6秒 (18.0 MB/s)           
   正在选中未选择的软件包 pigz。
   (正在读取数据库 ... 系统当前共安装有 199933 个文件和目录。)
   准备解压 .../0-pigz_2.6-1_amd64.deb  ...
   正在解压 pigz (2.6-1) ...
   正在选中未选择的软件包 containerd.io。
   准备解压 .../1-containerd.io_1.4.6-1_amd64.deb  ...
   正在解压 containerd.io (1.4.6-1) ...
   正在选中未选择的软件包 docker-ce-cli。
   准备解压 .../2-docker-ce-cli_5%3a20.10.7~3-0~ubuntu-hirsute_amd64.deb  ...
   正在解压 docker-ce-cli (5:20.10.7~3-0~ubuntu-hirsute) ...
   正在选中未选择的软件包 docker-ce。
   准备解压 .../3-docker-ce_5%3a20.10.7~3-0~ubuntu-hirsute_amd64.deb  ...
   正在解压 docker-ce (5:20.10.7~3-0~ubuntu-hirsute) ...
   正在选中未选择的软件包 docker-ce-rootless-extras。
   准备解压 .../4-docker-ce-rootless-extras_5%3a20.10.7~3-0~ubuntu-hirsute_amd64.deb  ...
   正在解压 docker-ce-rootless-extras (5:20.10.7~3-0~ubuntu-hirsute) ...
   正在选中未选择的软件包 docker-scan-plugin。
   准备解压 .../5-docker-scan-plugin_0.8.0~ubuntu-hirsute_amd64.deb  ...
   正在解压 docker-scan-plugin (0.8.0~ubuntu-hirsute) ...
   正在选中未选择的软件包 libslirp0:amd64。
   准备解压 .../6-libslirp0_4.4.0-1_amd64.deb  ...
   正在解压 libslirp0:amd64 (4.4.0-1) ...
   正在选中未选择的软件包 slirp4netns。
   准备解压 .../7-slirp4netns_1.0.1-1_amd64.deb  ...
   正在解压 slirp4netns (1.0.1-1) ...
   正在设置 docker-scan-plugin (0.8.0~ubuntu-hirsute) ...
   正在设置 containerd.io (1.4.6-1) ...
   Created symlink /etc/systemd/system/multi-user.target.wants/containerd.service → /lib/systemd/system/containerd.service.
   正在设置 docker-ce-cli (5:20.10.7~3-0~ubuntu-hirsute) ...
   正在设置 libslirp0:amd64 (4.4.0-1) ...
   正在设置 pigz (2.6-1) ...
   正在设置 docker-ce-rootless-extras (5:20.10.7~3-0~ubuntu-hirsute) ...
   正在设置 slirp4netns (1.0.1-1) ...
   正在设置 docker-ce (5:20.10.7~3-0~ubuntu-hirsute) ...
   Created symlink /etc/systemd/system/multi-user.target.wants/docker.service → /lib/systemd/system/docker.service.
   Created symlink /etc/systemd/system/sockets.target.wants/docker.socket → /lib/systemd/system/docker.socket.
   正在处理用于 man-db (2.9.4-2) 的触发器 ...
   正在处理用于 libc-bin (2.33-0ubuntu5) 的触发器 ...
   ```

2. 想要安装特定版本，先查看有哪些版本可以安装，然后进行选择：

   1. 列出仓库中可以安装的版本

      ```shell
      $ apt-cache madison docker-ce
      ```

      ```
      docker-ce | 5:20.10.7~3-0~ubuntu-hirsute | https://download.docker.com/linux/ubuntu hirsute/stable amd64 Packages
       docker-ce | 5:20.10.6~3-0~ubuntu-hirsute | https://download.docker.com/linux/ubuntu hirsute/stable amd64 Packages
      ```

   2. 比如我们可以用版本字符串 `5:18.09.1~3-0~ubuntu-xenial`安装特定的版本：

      ```shell
      $  sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
      ```

   3. 我们可以通过运行 `hello-world` 镜像来验证 Docker 引擎已经正确安装。

      ```shell
      $ sudo docker run hello-world
      ```

      这行命令会下在一个测试镜像，并在 container 里运行，它会打印一个信息性消息然后退出。

      ```
      Unable to find image 'hello-world:latest' locally
      latest: Pulling from library/hello-world
      b8dfde127a29: Pull complete 
      Digest: sha256:5122f6204b6a3596e048758cabba3c46b1c937a46b5be6225b835d091b90e46c
      Status: Downloaded newer image for hello-world:latest
      
      Hello from Docker!
      This message shows that your installation appears to be working correctly.
      
      To generate this message, Docker took the following steps:
       1. The Docker client contacted the Docker daemon.
       2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
          (amd64)
       3. The Docker daemon created a new container from that image which runs the
          executable that produces the output you are currently reading.
       4. The Docker daemon streamed that output to the Docker client, which sent it
          to your terminal.
      
      To try something more ambitious, you can run an Ubuntu container with:
       $ docker run -it ubuntu bash
      
      Share images, automate workflows, and more with a free Docker ID:
       https://hub.docker.com/
      
      For more examples and ideas, visit:
       https://docs.docker.com/get-started/
      ```

#### 通过安装包安装

1. 去 https://download.docker.com/linux/ubuntu/dists/ 下载需要的包

2. 使用类似于如下的命令安装即可：

   ```shell
   $ sudo dpkg -i /path/to/package.deb
   ```

3. 同样适用 `hello-world` 镜像验证是否安装成功

   ```shell
   $ sudo docker run hello-world
   ```

#### 升级 Docker 引擎

下载最新的安装包升级即可。

#### 使用便捷脚本安装

```shell
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
```

#### 安装之前的 releases 版本

```shell
$ curl -fsSL https://test.docker.com -o test-docker.sh
$ sudo sh test-docker.sh
```

#### 使用便捷脚本后升级 Docker

必须单独升级，不要重复运行脚本。

### 卸载 Docker 引擎

1. 卸载 Docker 引擎，CLI，Containerd 包：

   ```shell
   $ sudo apt-get purge docker-ce docker-ce-cli containerd.io
   ```

   ```
   正在读取软件包列表... 完成
   正在分析软件包的依赖关系树... 完成
   正在读取状态信息... 完成                 
   下列软件包是自动安装的并且现在不需要了：
     docker-ce-rootless-extras docker-scan-plugin libslirp0 pigz slirp4netns
   使用'sudo apt autoremove'来卸载它(它们)。
   下列软件包将被【卸载】：
     containerd.io* docker-ce* docker-ce-cli*
   升级了 0 个软件包，新安装了 0 个软件包，要卸载 3 个软件包，有 9 个软件包未被升级。
   解压缩后将会空出 426 MB 的空间。
   您希望继续执行吗？ [Y/n] y
   (正在读取数据库 ... 系统当前共安装有 200188 个文件和目录。)
   正在卸载 docker-ce (5:20.10.7~3-0~ubuntu-hirsute) ...
   Warning: Stopping docker.service, but it can still be activated by:
     docker.socket
   正在卸载 containerd.io (1.4.6-1) ...
   正在卸载 docker-ce-cli (5:20.10.7~3-0~ubuntu-hirsute) ...
   正在处理用于 man-db (2.9.4-2) 的触发器 ...
   (正在读取数据库 ... 系统当前共安装有 199970 个文件和目录。)
   正在清除 docker-ce (5:20.10.7~3-0~ubuntu-hirsute) 的配置文件 ...
   正在清除 containerd.io (1.4.6-1) 的配置文件 ...
   ```

2. Images, containers, volumes, 或者自定义配置文件不会被自动删除，需要手动删除。

   ```shell
   $ sudo rm -rf /var/lib/docker
   $ sudo rm -rf /var/lib/containerd
   ```
   通过打印可以看到 `/var/lib/docker` 目录下仍有残留
   ```shell
   sudo ls /var/lib/docker
   ```

   ```
   buildkit    image    overlay2  runtimes  tmp	volumes
   containers  network  plugins   swarm	 trust
   ```
   删除 `/var/lib/docker` 目录
   ```shell
    sudo rm -rf /var/lib/docker
   ```
   验证是否删除成功
   ```
   ls: 无法访问 '/var/lib/docker': 没有那个文件或目录
   ```
   通过打印可以看到 `/var/lib/containerd` 目录下仍有残留
   ```shell
   sudo ls /var/lib/containerd
   ```

   ```
   io.containerd.content.v1.content  io.containerd.snapshotter.v1.btrfs
   io.containerd.metadata.v1.bolt	  io.containerd.snapshotter.v1.native
   io.containerd.runtime.v1.linux	  io.containerd.snapshotter.v1.overlayfs
   io.containerd.runtime.v2.task	  tmpmounts
   ```
   删除 `/var/lib/containerd` 目录
   ```shell
   sudo rm -rf /var/lib/containerd
   ```
   验证是否删除成功
   ```
   ls: 无法访问 '/var/lib/containerd': 没有那个文件或目录
   ```

您必须手动删除任何已编辑的配置文件。