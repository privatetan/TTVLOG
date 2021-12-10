# SpringBoot构建脚本

## 1、项目运行脚本go.sh

```bash
#!/usr/bin/env bash

target=$1
port=$2
projectName="springboot4study"
health_url="http://127.0.0.1:$port/actuator/health"

echo "target = "$target

case $target in
     dev)
       pid=`ps -ef|grep 'java'|grep $projectName|grep $port|awk '{print $2}'`
       
```


