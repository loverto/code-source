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


## 解决docker容器无法访问宿主机端口的问题 No route to host 处理方法

请顺序运行以下命令：

```shell
nmcli connection modify docker0 connection.zone trusted

systemctl stop NetworkManager.service
firewall-cmd --permanent --zone=trusted --change-interface=docker0
systemctl start NetworkManager.service
nmcli connection modify docker0 connection.zone trusted
systemctl restart docker.service
```


https://blog.csdn.net/iouczp/article/details/80300500?utm_source=blogkpcl4
