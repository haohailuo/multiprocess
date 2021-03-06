# multiprocess [[readme in english]](README.en.md)
* 基于swoole的脚本管理，用于多进程和守护进程管理；
* 可轻松让普通PHP脚本变守护进程和多进程执行；
* 进程个数可配置，可以根据配置一次性执行多条命令；
* 子进程异常退出时,主进程收到信号，自动拉起重新执行；
* 支持子进程平滑退出，防止重启服务对业务造成影响；

## 1. 场景

* PHP需要跑一个或多个cli脚本消费队列（常驻）
* 实现脚本退出后自动拉起，防止消费队列不工作，影响业务
* 其实supervisor可以轻松做个事情，这个只是PHP的另一种实现，不需要换技术栈

## 2. 流程图
![流程图](flow.png)


## 3. 安装
* git clone https://github.com/kcloze/multiprocess.git
* composer install
* 根据自己业务配置,修改config.php


## 4. 配置实例
* 一次性执行多个命令
```
    'logPath'   => __DIR__ . '/log',
    'exec'      => [
        [
            'name'      => 'kcloze-test-1',
            'bin'       => '/usr/bin/php',
            'binArgs'   => [__DIR__ . '/test/test.php', 'oop', '123'],
            'workNum'   => 3,
        ],
        [
            'name'      => 'kcloze-test-2',
            'bin'       => '/usr/bin/php',
            'binArgs'   => [__DIR__ . '/test/test2.php', 'oop', '456'],
            'workNum'   => 5,
        ],
    ],

```
## 5. 运行

### 5.1 启动
* chmod -R u+r log/
* php multiprocess.php start >> log/system.log 2>&1
### 5.2 平滑停止服务，根据子进程执行时间等待所有服务停止
* php multiprocess.php stop
### 5.3 强制停止服务[慎用]
* php multiprocess.php exit
### 5.4 强制重启
* php multiprocess.php restart >> log/system.log 2>&1
### 5.5 监控
* ps -ef|grep 'multi-process'

### 5.6 启动参数说明
```
NAME
      php multiprocess - manage multiprocess

SYNOPSIS
      php multiprocess command [options]
          Manage multiprocess daemons.


WORKFLOWS


      help [command]
      Show this help, or workflow help for command.


      restart
      Stop, then start multiprocess master and workers.

      start
      Start multiprocess master and workers.

      stop
      Wait all running workers smooth exit, please check multiprocess status for a while.

      exit
      Kill all running workers and master PIDs.

```



## 6. 系统状态

![监控图](monitor.png)

## 7. change log

#### 2017-11-30
* 彻底重构v2版本
* 增加exit启动参数，默认stop等待子进程平滑退出


## 8. 感谢

* [swoole](http://www.swoole.com/)

## 9. 联系

qq群：141059677

