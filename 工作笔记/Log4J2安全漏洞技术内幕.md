# Log4J2安全漏洞技术内幕

## 背景

2021.12.09夜， log4j 爆发了史诗级漏洞，全球互联网公司大多都被殃及。

经奇安信CERT验证，Apache Struts2、Apache Solr、Apache Druid、Apache Flink等众多组件与大型应用均受影响，**鉴于此漏洞危害巨大，利用门槛极低，奇安信CERT建议用户尽快参考缓解方案阻止漏洞攻击。**

## 导致原因

Apache Log4j 是 Apache 的一个开源项目，通过定义每一条日志信息的级别，能够更加细致地控制日志生成过程。经过分析，Log4j-2中存在JNDI注入漏洞，当程序将用户输入的数据进行日志记录时，即可触发此漏洞，成功利用此漏洞可以在目标服务器上执行任意代码。

影响版本：2.0 <= Log4j -2 <= 2.14.1

影响判断方式：用户只需排查Java应用是否引入 log4j-api , log4j-core 两个jar。若存在应用使用，极大可能会受到影响。

SpringBoot默认使用Logback框架，但是不排除第三方工具不使用该框架

## 解决方案

（1） jvm参数 -Dlog4j2.formatMsgNoLookups=true

（2） log4j2.formatMsgNoLookups=True

（3）系统环境变量 FORMAT_MESSAGES_PATTERN_DISABLE_LOOKUPS 设置为true

## JNDI注入与Java反序列化漏洞利用



## SpringCloud远程调用与Double远程调用的区别