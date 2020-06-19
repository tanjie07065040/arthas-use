# 关于arthas工具使用

## 简介

 [arthas官网学习地址](https://alibaba.github.io/arthas/)

`Arthas` 是Alibaba开源的Java诊断工具，深受开发者喜爱。

当你遇到以下类似问题而束手无策时，`Arthas`可以帮助你解决：

1. 这个类从哪个 jar 包加载的？为什么会报各种类相关的 Exception？
2. 我改的代码为什么没有执行到？难道是我没 commit？分支搞错了？
3. 遇到问题无法在线上 debug，难道只能通过加日志再重新发布吗？
4. 线上遇到某个用户的数据处理有问题，但线上同样无法 debug，线下无法重现！
5. 是否有一个全局视角来查看系统的运行状况？
6. 有什么办法可以监控到JVM的实时运行状态？
7. 怎么快速定位应用的热点，生成火焰图？

`Arthas`支持JDK 6+，支持Linux/Mac/Winodws，采用命令行交互模式，同时提供丰富的 `Tab` 自动补全功能，进一步方便进行问题的定位和诊断。



内容模块如下:

![Contents](..\arthas\images\1.png)



## 方式1-使用方法

### 基于JAR包的使用流程准备

1：首先下载一个arthas-boot.jar包，下载地址 [https://alibaba.github.io/arthas/arthas-boot.jar](https://links.jianshu.com/go?to=https%3A%2F%2Falibaba.github.io%2Farthas%2Farthas-boot.jar)

2：准备一个业务的JAR包，这里拿呼叫中心的附件服务作为例子讲解

3：win+r输入cmd命令 进入到业务服务JAR包位置（建议和arthas-boot.jar包放在同一个目录位置），同时检查Java的环境是否具备(如果不具备需要安装Java的环境以及配置环境变量)，如图：

![](..\arthas\images\1-3.png)

![](..\arthas\images\1-2.png)



4：启动业务服务  java -jar cad-attach-backend-3.0.3-SNAPSHOT.jar,如图

![](..\arthas\images\1-4.png)

服务启动完成后，在本地浏览器检查服务的swagger地址是不是能够正常访问



5：win+r 输入cmd然后进入到arthas-boot.jar包位置（建议和业务服务放在同一个目录位置），输入java -jar arthas-boot.jar命令，如图：

![](..\arthas\images\1-5.png)

这个时候就可以看到我们第四步操作启动的附件服务，输入1，<font color=red>这里注意选中后需要有外网条件，会从阿里服务器下载相应文件</font>

![](..\arthas\images\1-6.png)

这里在arthas上进入到附件服务的诊断目录下，根据上面提供的端口信息(已经测试上面3658和8563二个端口都可以访问，并且是指向同一个进程ID)，可以访问地址如图：

![](..\arthas\images\1-7.png)



### 常用分析命令使用

#### dashboard

#### tarce

1：trace跟踪主要是跟踪目前服务中具体的方法

![](..\arthas\images\1-8.png)

1. --skipJDKMethod false  是否跳过JDK内部方法 false不跳过  true标识跳过
2. com.gsafety.cad.attach.service.serviceimpl.AttachServiceImpl 是类AttachServiceImpl的package路径位置
3. batchUpload是类AttachServiceImpl中的一个方法

#### thread

显示当前所有的线程信息,如图:

![](..\arthas\images\1-10.png)

```ruby
说明
ID: Java级别的线程ID，注意这个ID不能跟jstack中的nativeID一一对应
NAME: 线程名
GROUP: 线程组名
PRIORITY: 线程优先级, 1~10之间的数字，越大表示优先级越高
STATE: 线程的状态
CPU%: 线程消耗的cpu占比，采样100ms，将所有线程在这100ms内的cpu使用量求和，再算出每个线程的cpu使用占比。
TIME: 线程运行总时间，数据格式为分：秒
INTERRUPTED: 线程当前的中断位状态
DAEMON: 是否是daemon线程

通过上述信息，可以帮助我们快速定位相关问题线程。
```

查看具体线程ID的内容，如图:

![](..\arthas\images\1-11.png)

#### watch

![](..\arthas\images\1-12.png)

#### logger

~~~
获取classloaderHash值
logger -n com.gs.systemmanager.controller

根据classloaderHash查看日志信息
logger -c [classloaderHash]
~~~

#### monitor

~~~
10 标识监听次数
com.gs.systemmanager.controller package地址
getAllUserInfo  方法名
monitor -c 10  com.gs.systemmanager.controller getAllUserInfo
~~~



#### help

#### history

是使用命令的历史记录，如图；

![](..\arthas\images\1-9.png)



