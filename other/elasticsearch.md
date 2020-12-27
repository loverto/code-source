# Es 线上问题记录

es单元测试时报：[FORBIDDEN/12/index read-only / allow delete (api)] - read only elasticsearch indices

ElasticSearch进入“只读”模式，节点无法更改


[官方文档](https://www.elastic.co/guide/en/elasticsearch/reference/current/disk-allocator.html)

手动操作解除只读模式
PUT 
```json
_settings
{
  "index": {
    "blocks": {
      "read_only_allow_delete": "false"
    }
  }
}
```

原因，es默认情况下，如果磁盘存储空间使用率超过95%，30秒后，将会锁定节点的写入状态，变为只读状态

## Elastic 检查 vm.max_map_count is too low

解决：

切换到root用户

执行命令：

sysctl -w vm.max_map_count=262144

查看结果：

sysctl -a|grep vm.max_map_count

显示：

vm.max_map_count = 262144



上述方法修改之后，如果重启虚拟机将失效，所以：

解决办法：

在   /etc/sysctl.conf文件最后添加一行

vm.max_map_count=262144

即可永久修改


max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

https://www.cnblogs.com/yidiandhappy/p/7714489.html


## 镜像中用本地文件夹做挂载卷，提示没有权限

1. 通过生命volume的方式来解决权限问题
