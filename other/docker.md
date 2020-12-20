# docker

## 查看已经运行的容器的启动参数

runlike

用runlike 的docker模式来启动

alias runlike="docker run --rm -v /var/run/docker.sock:/var/run/docker.sock assaflavie/runlike"

使用命令

runlike YOUR-CONTAINER

## window 下docker 挂在卷

```yaml
-v: /e/work/:/data/work
```

## Docker window 10 上数据目录的迁移  wsl 2

https://github.com/docker/for-win/issues/7348
