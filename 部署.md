#### 官网下载elas和kibana包，解压到本地
本文基于6.6.0版本

#### 1. elasticsearch 
* 机器参数设置
```
sudo vim /etc/sysctl.conf
# 添加一行配置
vm.max_map_count=262144

cd /etc/security/limits.d/
sudo touch elas-limits.conf
sudo vim elas-limits.conf
# 添加配置
* soft nofile 65536
* hard nofile 131072
* soft nproc 4096
* hard nproc 4096
```

* 配置
```
# 进入到es目录
cd ${ELAS_PATH}/config/
vim elasticsearch.yml
# 修改网络配置
network.host: 192.168.2.101
http.port: 9200
```

* 运行elas
```
./${ELAS_PATH}/bin/elasticsearch >es.log 2>es-err.log &
```

* 查看
```
http://192.168.2.101:9200/
http://192.168.2.101:9200/_cat
http://192.168.2.101:9200/_cat/health
```

#### 2. x-pack
这个版本好像自带x-pack，不用另外下载
```
./${ELAS_PATH}/bin/x-pack/setup-passwords auto
```

#### 3. kibana
* 配置
```
vim ${KIBANA_PATH}/config/kibana.yml
# 设置网络
server.port: 5601
server.host: "192.168.2.101"
# 设置elas地址
elasticsearch.hosts: ["http://192.168.2.101:9200"]
# 设置x-pack设置的elasticsearch密码
elasticsearch.username= ""
elasticsearch.password= ""
```

* 启动
```
./${KIBANA_PATH}/bin/kibana >kb.log 2>kb-err.log &
```