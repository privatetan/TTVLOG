# Node基础

## Node入门

1. ###### 执行Node文件

   命令：node  script.js

2. ###### Node的REPL模式

   REPL模式：read-eval-print loop，输入-求值-输出循环；

   运行无参的node命令，会启动一个JS的交互式Shell；

   两次Ctrl+C即可退出REPL模式。

3. ##### Node包管理工具：npm

   npm，是nodejs官方提供的第三方包管理工具

4. ###### Node多版本管理器：nvm

   nvm，是nodejs社区开发的多版本管理器，用于维护多个版本的nodejs实例；

   nvm，通常指creationix/nvm或者visionmedia/n；

   安装visionmedia/n命令：npm install -g n；

   使用指定版本运行命令：n use 版本号 script.js。

5. ###### 自动重启NodeJS工具：supervisor

   nodejs只有在第一次引用到某部分时才会解析脚本文件，之后会直接访问内存，避免重复载入；在开发时，修改代码后，必须重新运行nodejs才生效。

   supervisor，会监视代码的改动并自动重启nodejs；

   安装命令：npm install -g supervisor



## 异步式IO与事件编程

### 异步式IO

###### 阻塞与非阻塞

|     阻塞（同步式IO）      |    非阻塞（异步式IO）    |
| :-----------------------: | :----------------------: |
|    多线程实现高吞吐量     |    单线程实现高吞吐量    |
| 由操作系统调度使用多核CPU |   单进程绑定到单核CPU    |
|    难以充分使用CPU资源    |    可充分使用CPU资源     |
| 内存轨迹大，数据局部性弱  | 内存轨迹小，数据局部性强 |
|     符合线性编程思维      |    不符合线性编程思维    |

### 回调函数

Readfile.js

```javascript
var fs = require('fs');
fs.readFile('file.txt','utf-8',function(err,data){
  if(err){
    console.error(err);
  }else{
    console.log(data);
  }
});
console.log('end. ');
```

运行结果：

```
end.
Contents of the file.
```

fs.readFile方法接收3个参数：文件名、编码方式、回调函数。

nodejs中，并不是所有的API都提供了同步和异步版本，nodejs不鼓励使用同步IO。

### 事件

nodejs的异步IO操作完成时，都会发送一个事件到事件队列。

###### EventEmitter

在开发者看来，事件都是由EventEmitter对象提供。

###### 事件循环机制

事件循环：nodejs程序由事件循环开始，到事件循环结束，所有逻辑都是事件的回调函数。

在事件循环中，程序的入口就是事件循环第一个事件的回调函数。

nodejs的事件循环对开发者是不可见的，由libev库实现；

libev事件循环的每一次迭代，在nodejs中就是一次Tick，libev会不断检查是否有活动的、可提供检测的事件监听器，知道检测不到时才退出事件循环，进程结束。

## 模块与包

模块（module）和包（package）是nodejs的重要组成部分，两者没有本质区别，一般不作区分；

nodejs提供require函数来调用其他模块，模块都是基于文件的；

nodejs的模块和包机制的实现参照了CommonJS标准，但并未完全遵循。

### 模块（Module）

模块，是nodejs应用基本组成，文件和模块一一对应：一个nodejs文件就是一个模块。

###### 创建模块

exports：公开模块接口

###### 获取模块

require：获取模块接口

### 包（Package）

包，是在模块基础上更深一步的抽象，类似于java的包（package）；

包，通常是一些模块的集合，对模块完成抽象封装，并对外提供接口的函数库。

包将某个独立的功能封装起来，用于发布、更新、依赖管理和版本控制。

###### 文件夹的模块

最简单的包，就是一个作为文件夹的模块。

###### package.json

package.json 位于模块的目录下，用于定义包的属性。

###### package.json 属性说明

- name - 包名。
- version - 包的版本号。
- description - 包的描述。
- homepage - 包的官网 url 。
- author - 包的作者姓名。
- contributors - 包的其他贡献者姓名。
- dependencies - 依赖包列表。如果依赖包没有安装，npm 会自动将依赖包安装在 node_module 目录下。
- repository - 包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上。
- main - main 字段指定了程序的主入口文件，require('moduleName') 就会加载这个文件。这个字段的默认值是模块根目录下面的 index.js。
- keywords - 关键字。

## Node包管理器-npm

npm，是nodejs官方提供的第三方包管理工具

### 获取包

命令：npm [install/i] [package_name]

### 本地模式与全局模式

npm安装包的模式：本地模式和全局模式；

默认情况下，npm install，采用本地安装模式，并将包安装到当前项目的node_modules子目录下；

