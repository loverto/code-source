# P6spy 源码分析

p6spy 如何自己实现一个类似的工具？

## 加载配置文件

1. 获取配置文件URL路径

1. 从相对路径获取配置文件/从类路径获取配置文件

1. 检测最后修改时间（获取最后修改时间后，切记要关闭InputStream）

1. 获取配置文件中指定的编码（如果没有的话，则用默认值（file.encoding或者UTF-8））

1. 按指定编码加载properties配置文件

1. 转换properties为HashMap结构

1. 关闭输入流

## P6spy 如何实现重新加载配置文件的

1. 清理相关设置

a. 销毁配置项对象

b. 销毁Jmx注册信息

c. 销毁JdbcEvent监听工厂对应的配置

1. 重新初始化

## JdbcEventListenerFactory

1. synchronized 同步

双重检查

## JdbcEventListener

jdbc 操作事件

## P6ModuleManager 核心入口

属性

1. 配置项

1. 所有配置项

1. P6Factory

1. MBean注册Jmxs

1. 配置查询库

1. 当前实例

### 初始化P6ModuleManager

1. 打印启动日志

1. 注册配置项变更监听P6LogQuery日志（日志统一输出）

1. 注册模块P6SpyFactory

1. 显示加载驱动

1. 获取所有模块工厂，再次注册

1. 配置项库设置初始化完成状态

1. 注册JmxBean信息

1. 配置项初始化完毕调用后置

### 实现自动加载配置文件

1. 定时执行服务(ScheduledExecutorService)

1. 线程池调度

可以做定时任务的单线程调度线程池
Executors.newSingleThreadScheduledExecutor()

## P6LogQuery

1. 初始化，获取日志服务对象

打印日志统一入口设置