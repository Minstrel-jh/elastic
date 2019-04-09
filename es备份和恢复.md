## 配置共享文件仓库路径
在`elasticsearch.yml`中配置`path.repo`
```yaml
path.repo: ["/mount/backups", "/mount/longterm_backups"]
```

## 查看快照仓库
```
GET /_snapshot
GET /_snapshot/_all
```

## 建立快照仓库
```
PUT /_snapshot/my_backup
{
    "type": "fs",
    "settings": {
        "localtion": "路径（需要是path.repo中的路径或子目录）",
        "compress": true
    }
}
```

## 查看快照仓库信息
```
GET /_snapshot/my_backup
```

## 验证仓库
```
POST /_snapshot/my_backup/_verify
```
会显示集群中使用这个仓库的节点的访问有效性。

## 建立快照（快照是增量的）
下面的参数
* 会等待快照结束。
* 指定要快照的索引。
* 忽略无效的。
* 不包含全局状态
```
PUT /_snapshot/my_backup/snapshot_1?wait_for_completion=true
{
    "indices": "index_1,index_2",
    "ignore_unavailable": true,
    "include_global_state": false
}
```

按照天来生成
```
PUT /_snapshot/my_backup/%3Csnapshot-%7Bnow%2Fd%7D%3E
```

## 获取快照信息
```
GET /_snapshot/my_backup/snapshot_1
```

## 查询所有的快照
```
GET /_snapshot/my_backup/_all
```

## 删除快照
删除之前的快照，似乎不影响后面的快照的恢复。需要进一步的研究使用压缩的情况。
```
DELETE /_snapshot/my_backup/snapshot_2
```

## 删除仓库的注册
这里不会直接删除仓库中的文件，只是解除注册。
```
DELETE /_snapshot/my_backup
```

## 恢复快照
* 可以指定恢复哪些index。
* 可以重命名。
* 可以指定恢复的index的设置。
```
POST /_snapshot/my_backup/snapshot_1/_restore

# 复杂点的
POST /_snapshot/my_backup/snapshot_1/_restore
{
  "indices": "index_1,index_2",
  "ignore_unavailable": true,
  "include_global_state": true,
  "rename_pattern": "index_(.+)",
  "rename_replacement": "restored_index_$1"
}
```

## 查看快照的状态
```
# 所有快照的状态
GET /_snapshot/_status

# 特定快照的状态
GET /_snapshot/my_backup/_status
```