# Docker安装gogs

1. 拉取gogs镜像

   ```
   docker pull gogs/gogs
   ```

   

2. 创建gogs本地文件夹

   ```
   sudo mkdir /tantao/gogs
   ```

   

3. 给文件夹赋权限

   ```
   sudo chown -R ${USER} /tantao/gogs
   ```

   

4. 创建docker网络，便于与容器通信

   ```
   docker network create cicd_net
   ```

   

5. 启动gogs容器

   ```
   docker run -d --name=gogs --network cicd_net -p 8888:3000 -p 9999:22 -v /tantao/gogs:/data gogs/gogs
   ```

   

6. 访问gogs页面，设置、安装、登录git仓库

   a. 地址：http://120.128.155.144:8888 (如果是服务器，更改为服务器地址，并开通端口)

   b. 选择数据库， 将【域名】设置为：120.128.155.144（Docker宿主机ip），【ssh端口】设置为：9999（域名和ssh端口，用于ssh克隆代码），应用URL改为http://120.128.155.144:8888 (用于http克隆代码)

   c. 点击安装，创建git账号，登录git账号。

   d.点击页面右上角用户图标，选择《设置》-《SSH密钥》-《添加密钥》

   e.生成密钥

   ```
   cd .ssh
   ssh-keygen -t rsa -C "privatetan@gogs.com"
   cat cat id_rsa.pub
   ```

   f.将生成的密钥粘贴至《添加目录》页面的《密钥内容》，完成密钥添加

   g.创建git仓库

   h.拉取项目

7. 参考博客

   [gogs安装与说明（docker）](https://www.cnblogs.com/shanfeng1000/p/14622319.html)



## 【问题解决】

##### 使用ssh克隆代码，报错“Permission denied”

解决：

在.ssh/目录下生成config文件

```
vim ~.ssh/config
```

内容：

```
Host 150.158.55.44
    Port 9999
    IdentityFile ~/.ssh/tencent_ssh
    User privatetan
```



## 搭建Docker的node服务



1. 编写Dockerfile

   ```
   FROM node
   
   WORKDIR /tantao/cloud/node
   
   COPY package*.json ./
   
   RUN npm install --only=production
   
   COPY . .
   
   CMD ["node","app.js"]
   ```

   

2. 在宿主机构建node服务的docker镜像

   ```
   docker build -t cicd_node .
   ```

   

3. 启动docker镜像容器

   ````shell
   docker run --name=cicdnode -d -p 8080:8080 cicd_node #端口与node服务端口一致
   ````

   
