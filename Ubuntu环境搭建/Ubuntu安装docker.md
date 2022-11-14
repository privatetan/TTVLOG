# Ubuntu安装Docker

ubuntu版本：20.04 +

## 安装

##### 1. 更新软件包索引

```shell
sudo apt update
```

##### 2. 安装必要的依赖软件，来添加一个新的 HTTPS 软件源

```shell
sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
```

##### 3. 导入源仓库的 GPG key

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

##### 4. 添加Docker APT 软件源

```shell
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

##### 5. 安装Docker

- 安装最新版本

  ```shell
  sudo apt install docker-ce docker-ce-cli containerd.io
  ```

- 安装指定版本

  a. 列出 Docker 软件源中所有可用的版本

  ```shell
  apt list -a docker-ce
  ```

  b. 通过在软件包名后面添加版本`=<VERSION>`来安装指定版本

  ```shell
  sudo apt install docker-ce=<VERSION> docker-ce-cli=<VERSION> containerd.io
  ```

##### 6. 验证安装

```shell
sudo systemctl status docker
```

##### 7.阻止 Docker 自动更新

```shell
sudo apt-mark hold docker-ce
```



## 卸载

##### 1. 停止所有正在运行的容器

```shell
docker container stop $(docker container ls -aq)
```

##### 2. 移除所有的 docker 对象

```shell
docker system prune -a --volumes
```

##### 3. 卸载docker

```shell
sudo apt purge docker-ce
sudo apt autoremove
```



## 非管理员用户执行Docker命令

默认情况下，只有 root 或者 有 sudo 权限的用户可以执行 Docker 命令。

想要以非 root 用户执行 Docker 命令，你需要将你的用户添加到 Docker 用户组，该用户组在 Docker CE 软件包安装过程中被创建。

想要这么做，输入：

```shell
sudo usermod -aG docker $USER
```

`$USER`是一个环境变量，代表当前用户名。

登出，并且重新登录，以便用户组会员信息刷新。