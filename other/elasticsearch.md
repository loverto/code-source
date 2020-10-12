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
