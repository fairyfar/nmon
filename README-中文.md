本项目主要用于修复`nmon`的Bug以及增强功能。

# 1. Bug修复



## 1.1. BUG1：工作于 `ralfmode` 时的主进程初始化同步问题

### 问题

当`nmon`工作在`ralfmode`时（使用参数`-p`），`fork`出来的子进程没有初始化完，主进程就返回了。

某些情况下，例如，在我们的`docker`测试环境中，以下命令需要20秒才返回。

```
linux_bbbp("netstat -r", "/bin/netstat -r 2>/dev/null", WARNING);
```

这就导致这段时间没有任何采集数据，而外部调用者很难知道采样的实际开始时间。

### 解决方法

设置一个信号量，子进程在初始化完成后，`post`信号量。主进程收到信号量后才返回，并打印进程ID。

### 修复日志

已修复。

# 2. 原始项目信息

原始`nmon`项目官网：[http://nmon.sourceforge.net](http://nmon.sourceforge.net)

# 3. 开源协议

GNU General Public License version 3.0 (GPLv3)

Scripted by FairyFar. [www.200yi.com](http://www.200yi.com/)
